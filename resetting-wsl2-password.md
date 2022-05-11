# Resetting Your WSL2 User's Password

If you are having to run a command with `sudo ...`, and you've realized that you do not remember your user's password, follow these steps to reset it:

1. Open a Powershell or Command Prompt window. **Not an Ubuntu terminal.**

2. Run `wsl -l` to get a list your installed WSL distributions.

3. Locate the distribution that has the user you are trying to change the password for.

    <img width="213" alt="wsl-l" src="https://user-images.githubusercontent.com/99351305/167859579-4d612da3-3afa-4cad-b337-e2cc418b9ef2.PNG">

    The name of my distribution is `Ubuntu`. Yours may be `Ubuntu-20.04` or something different.

4. Run `wsl -d <Your-Distribution-Name> -u root` where `<Your-Distribution-Name>` is your distro name. The name is case-sensitive.

    **Ex:** `wsl -d Ubuntu -u root`

5. You are now logged into your distribution as the `root` user. Run `passwd <the-username-of-the-account-you-are-changing-the-password-for>`

    **Ex:** `passwd matt`

6. Enter your new password twice.

7. You can now close out of the window. Your account's password has been changed! 🎉
