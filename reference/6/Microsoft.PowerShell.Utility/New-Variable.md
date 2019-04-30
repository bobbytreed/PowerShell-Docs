---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
keywords: powershell,cmdlet
locale: en-us
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: http://go.microsoft.com/fwlink/?LinkId=821838
schema: 2.0.0
title: New-Variable
---

# New-Variable

## SYNOPSIS
Creates a new variable.

## SYNTAX

```
New-Variable [-Name] <String> [[-Value] <Object>] [-Description <String>] [-Option <ScopedItemOptions>]
 [-Visibility <SessionStateEntryVisibility>] [-Force] [-PassThru] [-Scope <String>] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

## DESCRIPTION
The **New-Variable** cmdlet creates a new variable in PowerShell.
You can assign a value to the variable while creating it or assign or change the value after it is created.

You can use the parameters of **New-Variable** to set the properties of the variable, set the scope of a variable, and determine whether variables are public or private.

Typically, you create a new variable by typing the variable name and its value, such as `$Var = 3`, but you can use the **New-Variable** cmdlet to use its parameters.

## EXAMPLES

### Example 1: Create a variable

This command creates a new variable named days. The command uses the `-PassThru` parameter to show
the new variable.

```powershell
New-Variable -Name days -PassThru
```

```Output
Name                           Value
----                           -----
days
```

### Example 2: Create a variable and assign it a value

This example creates a variable named zipcode and assigns it the value 98033. The second command
outputs the value of the new variable by preceding the variable name with the dollar sign `$`.

```powershell
New-Variable -Name "zipcode" -Value 98033
$zipcode
```

```Output
98033
```

### Example 3: Create a variable with the ReadOnly option

This example shows how to use the **ReadOnly** option of `New-Variable` to protect a variable from
being overwritten. When you try to assign the variable a new value, you see the
**VariableNotWritable** error, as shown below.

The `Set-Variable` cmdlet allows you to overwrite the read-only variable with the `-Force` parameter.

The last command in the example pipes the variable to the `Select-Object` cmdlet which shows that
the **ReadOnly** option is still set.

```powershell
New-Variable -Name Max -Value 256 -Option ReadOnly
$max = 128
```

```Output
Cannot overwrite variable Max because it is read-only or constant.
At line:2 char:1
+ $max = 128
+ ~~~~~~~~~~
    + CategoryInfo          : WriteError: (Max:String) [], SessionStateUnauthorizedAccessException
    + FullyQualifiedErrorId : VariableNotWritable
```

```powershell
Set-Variable -Name Max -Value 128 -Force
Get-Variable -Name Max | Select -Property *
```

### Example 4: Create a private variable

This example demonstrates the behavior of a private variable. Declaring a variable as **Private**
hides the variable from other scopes. The [Invoke-Command](Invoke-Command.md) cmdlet is used to
execute the **ScriptBlocks** in the example.

The first command sequence declares a variable called `$test`, and then executes a **ScriptBlock**
which attempts to output the value of `$test`. Because `$test` was defined in a parent scope, the
**ScriptBlock** can access the value and displays it.

The second command sequence is the same as the first, except that the `$test` variable is declared
as **Private** with the `New-Variable` cmdlet. The `-Force` parameter is used to overwrite the
previously declared `$test` variable. In the second command sequence, the **ScriptBlock**
is unable to display the value of `$test` because it was declared with **Private** visibility.

For more information on scopes, see [about_Scopes](about_Scopes.md).

```powershell
New-Variable -Name test -Value "Can you see me?"
Invoke-Command -ScriptBlock { $test }
```

```Output
Can you see me?
```

```powershell

```

### Example 5: Create a variable with a space
```
PS C:\> New-Variable -Name 'with space' -Value 'abc123xyz'

PS C:\> Get-Variable -Name 'with space'

Name                           Value
----                           -----
with space                     abc123xyz

PS C:\> ${with space}
abc123xyz
```

This command demonstrates that variables with spaces can be created.
The variables can be accessed using the **Get-Variable** cmdlet or directly by delimiting a variable with braces.

## PARAMETERS

### -Description
Specifies a description of the variable.

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force
Indicates that the cmdlet creates a variable with the same name as an existing read-only variable.

By default, you can overwrite a variable unless the variable has an option value of ReadOnly or Constant.
For more information, see the *Option* parameter.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name
Specifies a name for the new variable.

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Option
Specifies the value of the **Options** property of the variable.The acceptable values for this parameter are:

- None.
Sets no options.
(None is the default.)
- ReadOnly.
Can be deleted.
Cannot be changed, except by using the *Force* parameter.
- Private.
The variable is available only in the current scope.
- AllScope.
The variable is copied to any new scopes that are created.
- Constant.
Cannot be deleted or changed.
Constant is valid only when you are creating a variable.
You cannot change the options of an existing variable to Constant.

To see the **Options** property of all variables in the session, type `Get-Variable | Format-Table -Property name, options -autosize`.

```yaml
Type: ScopedItemOptions
Parameter Sets: (All)
Aliases:
Accepted values: None, ReadOnly, Constant, Private, AllScope, Unspecified

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PassThru
Returns an object representing the item with which you are working.
By default, this cmdlet does not generate any output.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Scope
Specifies the scope of the new variable.
The acceptable values for this parameter are:

- Global.
Variables created in the global scope are accessible everywhere in a PowerShell process.
- Local.
The local scope refers to the current scope, this can be any scope depending on the context.
- Script.
Variables created in the script scope are accessible only within the script file or module they are created in.
- Private.
Variables created in the private scope cannot be accessed outside the scope they exist in.
You can use private scope to create a private version of an item with the same name in another scope.
- A number relative to the current scope (0 through the number of scopes, where 0 is the current scope, 1 is its parent, 2 the parent of the parent scope, and so on).
Negative numbers cannot be used.

Local is the default scope when the scope parameter is not specified.

For more information, see about_Scopes.

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Value
Specifies the initial value of the variable.

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -Visibility
Determines whether the variable is visible outside of the session in which it was created.
This parameter is designed for  use in scripts and commands that will be delivered to other users.
The acceptable values for this parameter are:

- Public.
The variable is visible.
(Public is the default.)
- Private.
The variable is not visible.

When a variable is private, it does not appear in lists of variables, such as those returned by Get-Variable, or in displays of the Variable: drive.
Commands to read or change the value of a private variable return an error.
However, the user can run commands that use a private variable if the commands were written in the session in which the variable was defined.

```yaml
Type: SessionStateEntryVisibility
Parameter Sets: (All)
Aliases:
Accepted values: Public, Private

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Confirm
Prompts you for confirmation before running the cmdlet.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf
Shows what would happen if the cmdlet runs.
The cmdlet is not run.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### CommonParameters
This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable. For more information, see about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

## INPUTS

### System.Object
You can pipe a value to **New-Variable**.

## OUTPUTS

### None or System.Management.Automation.PSVariable
When you use the *PassThru* parameter, **New-Variable** generates a **System.Management.Automation.PSVariable** object representing the new variable.
Otherwise, this cmdlet does not generate any output.

## NOTES

## RELATED LINKS

[Clear-Variable](Clear-Variable.md)

[Get-Variable](Get-Variable.md)

[Remove-Variable](Remove-Variable.md)

[Set-Variable](Set-Variable.md)
