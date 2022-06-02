# Setting Up an RDP Server in WSL2+Ubuntu

The saga continues! If you'd like to be able to use Remote Desktop Connection (mstsc.exe) to connect to your WSL2 Ubuntu environment and run GUI applications, you can follow these steps:

0. Update your Ubuntu system's packages before proceeding

`sudo apt-get update && sudo apt-get upgrade -y`

1. Install a Desktop Environment (DE)

`sudo apt-get install -y xfce4 xfce4-goodies`

When prompted for a desktop manager, select **LightDM**.

(feel free to use a different DE if you want)

2. Install the Mesa Driver and Extras:

`sudo apt-get install mesa-utils mesa-vulkan-drivers mesa-vdpau-drivers mesa-va-drivers`

3. Run each of the lines below separately:
```
sudo apt-get install xrdp

sudo cp /etc/xrdp/xrdp.ini /etc/xrdp/xrdp.ini.bak

sudo sed -i 's/3389/3390/g' /etc/xrdp/xrdp.ini

sudo sed -i 's/max_bpp=32/#max_bpp=32\nmax_bpp=128/g' /etc/xrdp/xrdp.ini

sudo sed -i 's/xserverbpp=24/#xserverbpp=24\nxserverbpp=128/g' /etc/xrdp/xrdp.ini
```

4. Edit the startwm.sh file so that the DE you installed in step 1 will run:

`sudo nano /etc/xrdp/startwm.sh`

5. Comment out the following lines with a # character:

```
test -x /etc/X11/Xsession && exec /etc/X11/Xsession
exec /bin/sh /etc/X11/Xsession
```

6. Add to the bottom of the file the command needed to start the DE you installed in step 1. If you went with XFCE, it'll be:

`startxfce4`

Your `startwm.sh` file should now look like this:

<img width="480" alt="startwm file after editing" src="https://user-images.githubusercontent.com/99351305/162492214-16a7aa9f-4855-465e-809a-ccf3435b5eee.PNG">


7. Add the new xrdp user to the ssl-cert group:

`sudo adduser xrdp ssl-cert`

If you don't do this you'll get a security error whenever you try to connect later.

8. Enable dbus (run one line at a time):
```
sudo systemctl enable dbus

sudo /etc/init.d/dbus start
```

9. Start up the xrdp server:

`sudo /etc/init.d/xrdp start`

You should now be ready to connect!!

10. Open up Remote Desktop Connection (mstsc.exe) and connect to `localhost:3390`. Be sure to replace `your-ubuntu-username` with your actual Ubuntu username.

<img width="305" alt="Remote Desktop Connection window" src="https://user-images.githubusercontent.com/99351305/162492458-ecf6d128-defd-4bb5-9ffe-168f97f5ae71.PNG">

11. Click on 'Connect'. You will be prompted to accept the certificate that is already located on your system. Do so to continue. Afterwards you will be brought to an xrdp login page:

<img width="352" alt="xrdp-login" src="https://user-images.githubusercontent.com/99351305/162492883-a7c3a0f9-2edb-4c0d-9413-1de6d297f873.PNG">

12. Enter your Ubuntu user's password and click 'Ok'.

You should should now be able to see your Ubuntu desktop!

<img width="960" alt="rdp-success" src="https://user-images.githubusercontent.com/99351305/162492998-f6a89eec-ca3f-4dea-aa72-ce52cc146469.PNG">


# Accessing the RDP Server After a Restart

If you restart your machine or WSL, you will no longer be able to connect to your RDP server as it will no longer be running. 😱

For ease of starting it back up you can do one of the below steps:

### 1. You want to manually start the RDP server as needed (pick this to save on RAM):

Add this line to your runtime configuration file (`.bashrc`/`.zshrc`/etc):

`alias start-rdp="sudo systemctl enable dbus && sudo /etc/init.d/dbus start && sudo /etc/init.d/xrdp start"`

After reopening the terminal you can then type `start-rdp` whenever you want to run the server.

### 2. You want the RDP server to start whenever you fire up Ubuntu:

Add this to the bottom of your runtime configuration file (`.bashrc`/`.zshrc`/etc):

`sudo systemctl enable dbus && sudo /etc/init.d/dbus start && sudo /etc/init.d/xrdp start && clear`

The RDP server will now attempt to run every time you open a terminal. You will also be asked for your password every time you open a terminal since the above commands contain `sudo`. 😅

If desired, resolve it with the following:

Run `sudo visudo`

Add the following line to the bottom:

`YOUR-USERNAME ALL=(ALL) NOPASSWD: ALL`

This will make it so you will not be asked for a password whenever using `sudo` for any command. Feel free to replace the last `ALL` with the paths to the executables in the commands you've added to your runtime configuration if you want to be more granular about it.
