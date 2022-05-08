# Powershell
Powershell is the Windows Scripting Language and shell environment that is built using the .NET framework.
output of cmdlets(powershell command) is object, making it object oriented. As it is an object, you can perform other actions on it.

The normal format of a _cmdlet_ is represented using **Verb-Noun**; for example the _cmdlet_ to list commands is called `Get-Command`.
Common verbs to use include:

-   Get
-   Start
-   Stop 
-   Read
-   Write
-   New
-   Out

### Get-Help
Get-Help displays information about a _cmdlet._ To get help about a particular command, run the following:
`Get-Help Command-Name`

You can also understand how exactly to use the command by passing in the
`-examples` flag.

### Get-Command
Get-Command gets all the _cmdlets_ installed on the current Computer. The great thing about this _cmdlet_ is that it allows for pattern matching like the following

`Get-Command Verb-*` or `Get-Command *-Noun`

### Object manipulation
As every output of cmdlets is object, it can be passed to other cmdlets with pipeline `|` .
The object also have methods and properties. You can view these details with:
`Verb-Noun | Get-Member`
For example: `Get-Command | Get-Member -MemberType Method`

### Creating Objects From Previous cmdlets
One way of manipulating objects is pulling out the properties from the output of a cmdlet and creating a new object. This is done using the `Select-Object` _cmdlet._
For example: `Get-ChildItem | Select-Object -Property Mode, Name`
You can also use the following flags to select particular information:
-   first - gets the first x object
-   last - gets the last x object
-   unique - shows the unique objects
-   skip - skips x objects
### Filtering Objects
When retrieving output objects, you may want to select objects that match a very specific value. You can do this using the `Where-Object` to filter based on the value of properties.
The general format of the using this _cmdlet_ is 

`Verb-Noun | Where-Object -Property PropertyName -operator Value`

`Verb-Noun | Where-Object {$_.PropertyName -operator Value}`
The second version uses the $_ operator to iterate through every object passed to the Where-Object cmdlet.

Where `-operator` is a list of the following operators:
-   -Contains: if any item in the property value is an exact match for the specified value
-   -EQ: if the property value is the same as the specified value
-   -GT: if the property value is greater than the specified value
-  -Match: Used for True/False expression
For a full list of operators, use [this](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/where-object?view=powershell-6) link.
Example `Get-Service | Where-Object -Property Status -eq Stopped`

### Sort Object
When a _cmdlet_ outputs a lot of information, you may need to sort it to extract the information more efficiently. You do this by pipe lining the output of a _cmdlet_ to the `Sort-Object` _cmdlet_.

The format of the command would be
`Verb-Noun | Sort-Object`

Example: `Get-ChildItem | Sort-Object`

### Get-ChildItem
same as ls, list current directory
```PowerShell
Get-ChildItem -File -Hidden -ErrorAction SilentlyContinue

#can be used to find files
Get-ChildItem C:\ -include *interesting-file.txt* -recurse -ErrorAction SilentlyContinue

# Find Files containing this string (API_KEY in this example)
Get-ChildItem C:\* -Recurse | Select-String -pattern API_KEY
```
#### options
- `-Path` specifies path, accept wildcard
- `-File/ -Directory` get a list of file/directory
- `-Filter` Specifies a filter to qualify the path parameter
- `-Recurse` If used with directory, list all the child also.
- `-ErrorAction SilentlyContinue` specifies what action if error is encountered, in this case silently continue
### Get-Content
read content of a file like cat
```PowerShell
Get-Content -Path file.txt
Get-Content -Path file.txt | Measure-Object -Word
(Get-Content 1.txt)[6991]
```
The `()[blabla]` format is used to get the nth index from the result, in this case the 6991th strings from the 1.txt file
#### options
- `Measure-Object -Word` used for inspectiong how many words are in the file


### Location

```PowerShell
#Change directories
Set-Location -Path c:\users\administrator\desktop
# Current working directory (pwd)
Get-Location
# Check if the path exist
Get-Location -Path "C:\Users\Administrator\Documents\Passwords"
```

### Select-String
search particular file for a pattern
```PowerShell
Select-String -Path 'c:\users\administrator\desktop' -Pattern '*.pdf'
```

### Get-Help
Show help for a particular cmdlets(powershell command)
```PowerShell
Get-Help Select-String
```

### Get-FileHash
Hash the file speciied
```PowerShell
Get-FileHash -Algorithm MD5 file.txt
```

### Measure
Give some statistical value about the object
```PowerShell
# count all the command installed where CommandType = Cmdlet
Get-Command | Where-Object -Property CommandType -eq cmdlet | measure
```

### Invoke-WebRequest
Make a request to a webserver, like curl
```PowerShell
Invoke-WebRequest -URI http://www.google.com
```

### Decode b64
`certutil.exe -decode "C:\Users\Administrator\Desktop\b64.txt" decode.txt`

### Get-LocalUser
List local users

```PowerShell
#By SID
Get-LocalUser -SID "S-1-5-21-1394777289-3961777894-1791813945-501"

```

### Get-LocalGroup
List local group

### Get-NetIPAddress
Get IP Address from all interface

### Get-NetTCPConnection
List all connection
```PowerShell
#List all listening port
Get-NetTCPConnection | Where-Object -Property State -Match Listen
```

### Get-Hotfix
List all patches applied
```PowerShell
#By ID
Get-Hotfix -Id KB4023834
```

### Get-Process
List all proccess

### Get-ScheduleTask
List all scheduled task
```Powershell
#List by name
Get-ScheduleTask -TaskName new-sched-task
```

### Get-Acl
Get the owner of file/folder

## Scripting
https://learnxinyminutes.com/docs/powershell/