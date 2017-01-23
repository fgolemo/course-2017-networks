# Networks Course

First please clone this repository:

    git clone https://github.com/fgolemo/course-2017-networks.git
    
## Installing Qemu from source

(This guide assumes Ubuntu/Debian)

First make sure you got the latest package list

    sudo apt-get update

If you previously experimented with qemu or if there is any chance that an old version is still installed on your system, remove it first - we're going to compile a fresh new one.

    sudo apt-get remove qemu

Next up grab some obligatory packages that we definitely need for building

    sudo apt-get install build-essential vde2 libattr1 libvirt0 socat wget

And the following packages have mixed urgency, but for my course I require you to install all of them. Some of them are for the graphical output, some for making sure we have networking support, etc.

    sudo apt-get install libglib2.0-dev libfdt-dev libpixman-1-dev zlib1g-dev libvde-dev libvdeplug-dev libnfs-dev libiscsi-dev libcap-dev libattr1-dev libsdl-dev

With everything prepared, grab the latest version of Qemu from their website (in my case v2.8.0)

    wget http://wiki.qemu-project.org/download/qemu-2.8.0.tar.bz2

...and unpack it

    tar xjvf qemu-2.8.0.tar.bz2
    cd qemu-2.8.0

Now this is the important part, before we compile Qemu, we have to make sure we configure it with at least the following options:

    ./configure --target-list=x86_64-softmmu --enable-kvm --enable-vde --enable-virtfs --enable-sdl

This should complete without any errors. Only if this goes through, you can actually compile Qemu. We need KVM for fast virtualization, VDE for networking, virtfs for god knows what (qemunet complains if it's missing) and SDL or curses for having a visual process for the virtual machines.

If that went well, we can finally compile and install the software:

    make
    sudo make install

Of course the compilation should not show you any errors. To test if the installation went through, please run the following command:

    qemu-system-x86_64 --version
    
This should display something like:

    QEMU emulator version 2.8.0
    Copyright (c) 2003-2016 Fabrice Bellard and the QEMU Project developers
    
# Installing QemuNet

To get QemuNet, first make sure you have subversion installed

    sudo apt-get install subversion
    
Then clone their repository

    svn checkout svn://scm.gforge.inria.fr/svnroot/qemunet/

This will create a new directory `qemunet` with a subfolder `trunk`. Please switch to that subfolder now.
    
    cd qemunet/trunk
   
Now that you have that installed, we can get started with the first course
