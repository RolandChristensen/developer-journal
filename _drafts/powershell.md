Learning powershell.

## Variables
`$VariableName = 99`  

They are not just for scripting. Variables can be defined in the console and used later.  

### Interpolation
Using double quotation marks allow you to use interpolation (displays the variable value)  
Use the back tick "\`" to display the variable name instead of its value.  
``Write-Host "`$VariableName = $VariableName``: Displays "$VariableName - 99"  

Single quotes will only print the variable name. Interpolation only works using double quotes.  
`Write-Host '$VariableName'`: Displays "$VariableName"  

$() allows you to interpolate calculations.  
``Write-Host "\`$VariableName + 1 = $($VariableName = 1)"``
