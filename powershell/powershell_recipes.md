Here you can find a collection of `PowerShell` recipes that can be used in day-to-day work

| Cmdlets                                                      | Explanation                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `ls \| Select-Object -Property Name \| Select-String .docx`    | Find all `.docx` files in the current directory              |
| `(Get-Process powershell \| select -First 1).Path`<br />`(Get-Command powershell.exe).Definition` | Get Absolute Path to PowerShell executable                   |
| `Expand-Archive c:\a.zip -DestinationPath c:\a`              | Extract archive                                              |
| `ipconfig /all > ipconfig_all.txt`                           | Save output of `cmdlet` to file                              |
| `Get-Process geckodriver* \| Stop-Process -PassThru`          | Find all processes using wildcard query and terminate them, output additional info |
| `Get-ChildItem env:`                                         | Get all environment variables                                |
| `cd (Get-Item Env:GROOVY_HOME \| select -ExpandProperty Value)` | Go to a directory from Environment Variable                  |
| `new-item README.md -ItemType file`                          | Create a new file in current directory                       |
| `Move-Item .\password\ D:\vcs\gitlab\`                       | Move directory to a new location                             |

### Find out what process locked the directory 

```powershell
IF((Test-Path -Path $FileOrFolderPath) -eq $false) {
    Write-Warning "File or directory does not exist."       
}
Else {
    $LockingProcess = CMD /C "openfiles /query /fo table | find /I ""$FileOrFolderPath"""
    Write-Host $LockingProcess
}
```

Credits: https://superuser.com/a/1203347/307030
