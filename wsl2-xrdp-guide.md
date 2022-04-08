The saga continues! If you'd like to be able to use Remote Desktop Connection (mstsc.exe) to connect to your WSL2 Ubuntu environment and run GUI applications, you can follow these steps:

1. Install a Desktop Environment (DE)

`sudo apt-get install -y xfce4 xfce4-goodies`

(feel free to use a different DE if you want)

2. Run each of the lines below separately:
```
sudo apt-get install xrdp

sudo cp /etc/xrdp/xrdp.ini /etc/xrdp/xrdp.ini.bak

sudo sed -i 's/3389/3390/g' /etc/xrdp/xrdp.ini

sudo sed -i 's/max_bpp=32/#max_bpp=32\nmax_bpp=128/g' /etc/xrdp/xrdp.ini

sudo sed -i 's/xserverbpp=24/#xserverbpp=24\nxserverbpp=128/g' /etc/xrdp/xrdp.ini
```

3. Edit the startwm.sh file so that the DE you installed in step 1 will run:

`sudo nano /etc/xrdp/startwm.sh`

4. Comment out the following lines with a # character:

```
test -x /etc/X11/Xsession && exec /etc/X11/Xsession
exec /bin/sh /etc/X11/Xsession
```

5. Add to the bottom of the file the command needed to start the DE you installed in step 1. If you went with XFCE, it'll be:

`startxfce4`

Your `startwm.sh` file should now look like this:

<img width="480" alt="startwm file after editing" src="https://user-images.githubusercontent.com/99351305/162492214-16a7aa9f-4855-465e-809a-ccf3435b5eee.PNG">


6. Add the new xrdp user to the ssl-cert group:

`sudo adduser xrdp ssl-cert`

If you don't do this you'll get a security error whenever you try to connect later.

7. Enable dbus (run one line at a time):
```
sudo systemctl enable dbus

sudo /etc/init.d/dbus start
```

8. Start up the xrdp server:

`sudo /etc/init.d/xrdp start`

You should now be ready to connect!!

9. Open up Remote Desktop Connection (mstsc.exe) and connect to `localhost:3390`. Be sure to replace `your-ubuntu-username` with your actual Ubuntu username.

<img width="305" alt="Remote Desktop Connection window" src="https://user-images.githubusercontent.com/99351305/162492458-ecf6d128-defd-4bb5-9ffe-168f97f5ae71.PNG">

10. Click on 'Connect' and you will be brought to an xrdp login page:

<img width="352" alt="xrdp-login" src="https://user-images.githubusercontent.com/99351305/162492883-a7c3a0f9-2edb-4c0d-9413-1de6d297f873.PNG">

11. Enter your Ubuntu user's password and click 'Ok'.

You should should now be able to see your Ubuntu desktop!

<img width="960" alt="rdp-success" src="https://user-images.githubusercontent.com/99351305/162492998-f6a89eec-ca3f-4dea-aa72-ce52cc146469.PNG">




