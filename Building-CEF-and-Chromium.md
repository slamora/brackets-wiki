The first thing you're going need to do is clone the brackets and brackets-shell repositories and build brackets-shell following the instructions from the brackets-shell repo.

Next you need to get the Depot_Tools and add it to your path

    cd ~/
    git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git

Next open your .bash_profile or .bashrc and add ~/depot_tools to your path:

    export PATH="$PATH":~/depot_tools

You also need to execute that line in bash or open a new terminal window.

Next you need to add googlecode to the list of trusted certificates for svn

    svn list https://sctp-refimpl.googlecode.com/svn/trunk/KERN/usrsctp/usrsctplib

Next you need to get gyp into a search path.  I just copied the one from ~/brackets-shell/gyp to ~/depot_tools. 

Next you need to start the process of getting the chromium and cef sources:

    cd ~/
    svn checkout http://chromiumembedded.googlecode.com/svn/branches/1547/cef3/automate automate
    cd ~/automate
    python automate.py --url http://chromiumembedded.googlecode.com/svn/branches/1547/cef3 --download-dir download --ninja-build --no-release-build --force-build --force-distrib
 
This will clone the chromium source and it may stop on an error because gclient hasn't been configured yet.  You need the download folder to configure it so you can do that now and restart it.

    cd ~/automate/download/chromium
    gclient config http://src.chromium.org/svn/releases/29.0.1547.22

Now restart the build
    cd ~/automate
    python automate.py --url http://chromiumembedded.googlecode.com/svn/branches/1547/cef3 --download-dir download --ninja-build --no-release-build --force-build --force-distrib

You should finish now but if you run into trouble from here try this:

    cd ~/automate
    python automate.py --url http://chromiumembedded.googlecode.com/svn/branches/1547/cef3 --download-dir download --ninja-build --no-release-build --force-build -–force-clean --force-distrib

Other flags (documented as we find and need them):

    
    --force-config 
    --force-update

I didn't need them but Ion suggested that you could use them if you absolutely need to clean everything and reset.