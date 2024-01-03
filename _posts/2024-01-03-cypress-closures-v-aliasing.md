---
title: "Cypress Closures (Nesting) vs. Aliasing"
date: 2024-01-03
---

By default, Cypress uses chaining and closures to gather values needed for testing.

```javascript
cy.get('[data-cy="first-name"]').invoke('text').then((firstNameText) => {
  cy.get('[data-cy="middle-name"]').invoke('text').then((middleNameText) => {
    cy.get('[data-cy="last-name"]').invoke('text').then((lastNameText) => {
      cy.get('#full-name').invoke('text').then((fullNameText) => {
        expect(firstNameText + ' ' + middleNameText + ' ' + lastNameText).to.equal(fullNameText)
      })
    })
  })
})
```

Coming from a structured procedural programming approach, where you gather data using functions and return them to a variable (`var x = getFirstName()`), you may find this frustrating.   

To avoid nesting use aliases simulating a procedural approach.

```javascript
cy.get('[data-cy="first-name"]').invoke('text').as('firstNameText')
cy.get('[data-cy="middle-name"]').invoke('text').as('middleNameText')
cy.get('[data-cy="last-name"]').invoke('text').as('lastNameText')

// Note that "function" is used instead of arrow notation.
// Arrow notation would mean that the "this" keyword would follow all nested arrows until it reached the outermost scope, where it would not find the alias.
// Using "function()" searches only the current block.
cy.get('#full-name').invoke('text').as('fullNameText').then(function () {
  expect(this.firstNameText + ' ' + this.middleNameText + ' ' + this.lastNameText).to.equal(this.fullNameText)
})
```

**Takeaway**: get used to using both ways.  
Reduce nesting when it improves readability, but use closures to ensure integrity of variable values.  

The internet has a lot of strong opinions, often with no data or logic to back it. There is no consensus.

Pros to closures:
* You get the value right before you use it and it is less likely to be unintentionally changed before you use it.

Cons to closure nesting:
* Excessive nesting is generally viewed as a code quality problem. It is assumed to reduce readability, leading to an increase in defects and longer maintenance times (increased technical debt).
* On small monitors, when you are viewing a side by side diff or coding using side by side, you will find yourself using the scroll bar more often slowing you down and decreasing readability.

Pros to aliases: 
* Reduced Nesting
* Fewer lines of code (mostly closing brackets and parentheses)

Cons to aliases:
* Aliases increase complexity. You need to use functions to set and get the variable and then chain off the result. Increased complexity is said to decrease code quality in the long run, increasing technical debt.
* Aliases are not as easily tracked in the IDE as a variable is in modern IDEs. IntelliSense and code navigation in VS Code makes finding references to variables and when they are set easy. Searching for alias names is a little more difficult.
