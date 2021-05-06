---
title: "GitHub Pages Notes"
date: 2021-05-05
---

This documentation is for creating a static web page on GitHub. A quick search will give you the official "GitHub Docs" documentation. I will only cover things that were not clear in the documentation and try to make things pithy for future use.

# Quick Guide for Creating a Static Site for a Repo
1. Edit the settings for the repo.
	- Click "Settings" on Top Control for Repo.
	- Click "Pages" on Left Hand Control.
	- Select the branch you want to use to publish the site. When you push changes to the branch it will automatically build the site.
	- Click "Save." 
2. Create a file called "index.html" for your site. (index.html, index.md will both work or Readme.md will be used if an index does not exist.)
	- Create one of the files above in the root of the repo.
3. Edit the file created above as you would any other static web site.
4. When you merge the changes the site will automatically build for you.
	
# Quick Guide for Creating a Static Blog
1. Edit the settings for the repo.
	- Click "Settings" on Top Control for Repo.
	- Click "Pages" on Left Hand Control.
	- Select the branch you want to use to publish the site. When you push changes to the branch it will automatically build the site.
	- Click "Save." 
2. Create a file called "index.md" for your site. (index.html, index.md will both work or Readme.md will be used if an index does not exist.)
	- Create one of the files above in the root of the repo.
3. Edit the file created above and add the following.
	- YAML front matter for the page if you are using markdown index. These lines must be the first lines of the file. The title key will be displayed on the page, depending on the theme used. The theme will be defined in the _config.yml in another step. 
		```
		Example: 
		---
		title: "The title to display at the top of the page"
		---
		```
	- Add the content you want displayed.
4. Create the Jekyll configuration file. Jekyll is used to generate the web site. A search will give you documentation. All of this will be displayed on the page in predefined areas depending on the theme you define. 
	- Save a file as "_config.yml" in the root of the repo.
	- Add contents as needed. Search for options of Jekyll config file. Below is an example file to get you started.
```
		title: This will be displayed on the top of the blog.
		author: Your Name 
		email: YourEmail@example.com
		description: > # this means to ignore newlines until next key. AKA muliti-line formatting in YAML.
			This is a sentence.
			This is another sentence.
		# social links (Google for list of all social media types)
		github_username:  FirstLastName # Do NOT include the @ character!

		show_excerpts: true # set to false to remove excerpts on the homepage

		theme: minima # Search for list of themes, to see what is available. 
			      # You can also use the theme wizard on GitHub to see what they look like.
```
5. Create a blog post.
	- Create a folder in the root directory titled "_posts" to hold all blog posts.
	- Each blog post file should follow the naming convention "YYYY-MM-DD-title-with-hyphens.md" to work with Jekyll. YYYY (year), MM (month), DD (day), and the title of the page all with hyphens between words, instead of spaces.
	- The contents of the file must begin with YAML front matter as below.
	- Follow that with the contents of the blog post.
		```
		---
		title: "The title of the blog post"
		date: 2021-05-05
		---
		
		Blog, blog, blog
		Blah, blah, blah
		```
6. When you merge the changes the site will automatically build for you.

# Pitfalls
## Use the Title Front Matter Instead of a Header
The first problem I had when I created the index.md for a blog, I did not understand where the title and headers would be displayed on the page. You will need to keep the title lengths under a minimum number to display correctly depending on the theme. The YAML Front Matter does not display in the preview of the file when you are editing it, so there is a temptation to add a header to the top of the markdown file created. Resist the temptation. The title key will create a lovely header on the finished web page.

![Example Layout of Minima Theme](https://rolandchristensen.github.io/DeveloperJournal/images/2021-05-05-GitHub-Pages.png "Example of Using Title in YAML Front Matter and a Heading")

As noted in the screen shot the plug-in "jeckyll-titles-from-headings" is useful, but can cause problems. It will create a duplicate heading and title if you do not include a "title" key in the YAML front matter.

## How to Add an Image
The documentation found on the Jekyll site or GitHub pages does not seem to work. They say you should add an assets folder with images and reference the image with a relative path from the root directory. That is only half right at the time of writing of this post. Adding a folder to hold any assets you want to link to is correct. However, you need to create a full path to the GitHub project assets folder.
To get this to work you need to follow this pattern:
```
![Text Reader Text](https://{GitHub User Name}.github.io/{ProjectName}/assets/imageName.png "Hover Message")
```
This works for GitHub project sites.