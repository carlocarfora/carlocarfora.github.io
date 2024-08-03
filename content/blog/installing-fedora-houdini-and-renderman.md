+++
title = "Installing Fedora Houdini and Renderman"
date = 2019-12-16
[taxonomies]
  tags = ["linux", "fedora", "renderman"]
[extra]
  toc = false
+++


Installing Fedora Houdini and Renderman
16 Dec
This is a guide on setting up a system with Fedora, Houdini and Renderman. There were a few obstacles along the way and hopefully this can help anyone who might want to do the same thing. You may not have the same issues I did, but I had to do some extensive googling to find the answers to issues so hope this helps!

Installing Fedora with Optimus
Installed the vanilla Fedora 31 Workstation with GNOME. My laptop is an NVIDIA Optimus machine, and this has support out of the box once you install the proprietory NVIDIA drivers. It did not boot up initially though when trying to run the live USB; this is common with Optimus setups.

When booting, in the GRUB menu press e and add nouveau.modeset=0 to the bootup parameters. Put it just before the word quiet, not at the end of it all.

Once you're in Fedora, install the Free and Nonfree repos from [https://rpmfusion.org] then follow the guide here to get Optimus working.

Check it's all good with __NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia glxinfo | grep vendor. You'll need to start Houdini with this command to use the NVIDIA card, so wrap it up into an alias or bash script.

Installing Houdini
Once that's done, you can move on to installing Houdini. Grab the download, install as you would in the usual way. I had Houdini refusing to start because I was missing a couple of shared libraries, specifically libnsl and libGLU. I found and installed both of these libraries using the package manager. If you have anything else missing for some reason, you can probably find and install them using DNF.

I found that the license server service sesinetd was failing to start on boot. This is because of SELinux and the default security setup. The way that I got this working was to install a package called setroubleshoot. Once you have this installed, when you reboot on GNOME it'll pop up with an alert saying that the service sesinetd could not be started. It'll also give you the commands to fix this, it looks something like this (yours could be different so check the output rather than copy/pasting):

  ausearch -c 'sesinetd' --raw | audit2allow -M my-sesinetd
  semodule -X 300 -i my-sesinetd.pp
Make sure to run these as sudo and with the first command, you'll need to put sudo after the pipe (|) as well as audit2allow wants to run as sudo too.

Installing Renderman
To install Renderman NC, you need to download their installer. I had trouble running their installer to start with; Fedora will install it for you then you can then find it in /opt/pixar which is not immediately obvious.

When trying to run RenderManInstaller, it would crash with Segmentation Fault. Searchign the forums, I found a thread that mentions it uses a version of openssl that isn't installed by default. Fix this by installing the compat-openssl10-devel package using DNF. You can then proceed to install Renderman NC for your machine.

Once you have installed Renderman NC, you'll need to edit your houdini.env file which you can find usually in ~/houdini##.#. Add these lines to the file:

  # RENDERMAN ENV VARIABLES
  RMANTREE=/opt/pixar/RenderManProServer-23.0
  RFHTREE=/opt/pixar/RenderManForHoudini-23.0
  HOUDINI_PATH=$RFHTREE/18.0:&
If all went well, you can run Houdini and see Renderman is installed. In my case, when I tried to drop down a ris ROP and render something I got another error message about a missing library called libtinfo. To install this library, I had to install a package called ncurses-compat-libs from DNF.

After this, I was able to load Houdini and Renderman. I hope this helps anyone out there who might have had issues like I did!

Carlo
