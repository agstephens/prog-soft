In `powershell` (run as `administrator`), you can run the `cipher` command to safely overwrite all empty space under a Windows disk. E.g. to clean up and randomly overwrite everything on the `C:` drive that is not in use:

```
cipher /w:C:
```

Source: https://www.majorgeeks.com/content/page/how_to_securely_delete_files_in_windows_10.html

