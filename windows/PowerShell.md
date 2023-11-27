## Running Windows Anaconda "conda" in Windows PowerShell (NOT as Administrator):

If you are (stuck) on Windows, you might wish you could use a Linux Shell. 
Windows has a built-in tool called **PowerShell**, that can handle both DOS and Bash commands.

If you have installed Anaconda on Windows, you can access it within PowerShell by following these steps:

1. Start PowerShell from windows command menu (i.e. press the Windows key and start typing PowerShell):
- do NOT start it as Administrator

2. Run a specific command to set things up (and modify it based on where you have Anaconda installed):

Assuming Anaconda is installed at "C:\\ProgramData\\Anaconda3", run:

```
powershell -ExecutionPolicy ByPass -NoExit -Command "& 'C:\ProgramData\Anaconda3\shell\condabin\conda-hook.ps1' ; conda activate 'C:\ProgramData\Anaconda3' "
```

3. Now you can initialise PowerShell in conda:

```
conda init powershell
```

4. Close PowerShell.

5. Restart PowerShell (using the same approach as in (1)).

6. You should see your prompt starts with "(base)" - indicating your base conda env is already running.

`python` and `conda` should now be available at the command line.

### Changing the prompt in PowerShell
Change prompt

```
function prompt { "$ " }
```

### An equivalent to ".bashrc" or ".profile" in PowerShell
It is called `$profile`, e.g.:

```
notepad $profile
```

WARNING!!! The same profile file is used for admin and normal users!