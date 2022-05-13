# Limiting WSL2 Memory Usage

1. Create a file in your `%UserProfile%` folder on Windows named `.wslconfig`. Any configurations added to this file will be applied to all WSL distributions you have installed.
2. Open `.wslconfig` and add the following settings:
```
  [wsl2]
  memory=XGB
```
Replace `X` with the number of gigabytes of RAM you would like to allow WSL to use. I'd recommend starting with 8, if your PC can spare that amount, and seeing how that works.

3. Close out of all of your WSL terminals (including VS Code WSL windows- be sure to save your work!)
4. Open CMD or Powershell and then run `wsl --shutdown`
5. Re-open the windows you've closed in step 2 and enjoy not having as many lock-ups
