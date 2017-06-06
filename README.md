# SiX ROM #
[<center><img src="https://lh3.googleusercontent.com/-Rm8-kc-QUl8/WRs1h_QWlOI/AAAAAAABZNk/Zg5HQvh8ojsY_vHJsLyrs4tSL8nmH10TwCJoC/w530-h335-p-rw/Untitled9966op.png" height="100%" width="100%;"/></center>](https://plus.google.com/u/0/communities/108198916369516028494)

# Getting Started
---------------

To build SiX from source, you'll need to be familiar with
[Git and Repo](http://source.android.com/download/using-repo).

To initialize your local repository, use this command:

	repo init -u https://github.com/SiXROM/manifest.git -b n-mr2.1

Then to sync source, use this command:

	repo sync

After syncing is done, use these commands to build:

    1.) . build/envsetup.sh
    2.) brunch xxxx yyyy
    
    xxxx= device name aka shamu
    yyyy= build type (user,userdebug,eng)*

    *if no build type is specified "user" is default


# ************UPDATED ON 6-05-17************

Setup to build Android from source

Instructions:

# 1) Remove all traces of Java:

sudo apt-get remove openjdk-* icedtea-* icedtea6-*

# 2) Add OpenJDK7 PPA & Fetch: 

sudo add-apt-repository ppa:openjdk-r/ppa && sudo apt-get update

#3) Install all available updates:

sudo apt-get upgrade && sudo apt-get dist-upgrade

#4) Install OpenJDK7 and Android dependencies:

sudo apt-get install adb

sudo apt-get install fastboot

sudo apt-get install openjdk-7-jdk

sudo apt-get install git

sudo apt-get install ccache

sudo apt-get install automake

sudo apt-get install lzop

sudo apt-get install bison

sudo apt-get install gperf

sudo apt-get install build-essential

sudo apt-get install zip

sudo apt-get install curl

sudo apt-get install zlib1g-dev

sudo apt-get install zlib1g-dev:i386

sudo apt-get install g++-multilib

sudo apt-get install python-networkx

sudo apt-get install libxml2-utils

sudo apt-get install bzip2

sudo apt-get install libbz2-dev

sudo apt-get install libbz2-1.0

sudo apt-get install libghc-bzlib-dev

sudo apt-get install squashfs-tools

sudo apt-get install pngcrush

sudo apt-get install schedtool

sudo apt-get install dpkg-dev

sudo apt-get install liblz4-tool

sudo apt-get install make

sudo apt-get install optipng

sudo apt-get install maven

sudo apt-get install python-mako

sudo apt-get install python3-mako

sudo apt-get install python

sudo apt-get install python3

sudo apt-get install syslinux-utils

sudo apt-get install flex

sudo apt-get install lib32ncurses5-dev

sudo apt-get install libxml2-utils

sudo apt-get install xsltproc

sudo apt-get install google-android-build-tools-installer

# 5) Remove unnecessary packages:

sudo apt-get autoremove

# 6) Make a user accessible folder, and add it to path:

mkdir ~/bin && echo "export PATH=~/bin:$PATH" >> ~/.bashrc

# 7) Enable CCACHE:

echo "export USE_CCACHE=1" >> ~/.bashrc

# 8) Restart Bash:

source ~/.bashrc

# 9) Configure Git:

git config --global user.name "username"

git config --global user.email useremail@email.com

# 10) Enable Repo and put it in local $PATH folder:

curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo && chmod a+x ~/bin/repo

# 11) Make a source folder for your ROM and initialize repo:

mkdir ~/SiXROM

next

cd ~/SiXROM

now

repo init -u https://github.com/SiXROM/manifest.git -b n-mr2.1


# 12) Sync Repo:

repo sync

if you need to force sync

repo sync --force-sync

# 13) Build the ROM:

prebuilts/misc/linux-x86/ccache/ccache -M 50G

. build/envsetup.sh && brunch shamu userdebug

or the long way

. build/envsetup.sh

lunch

pick your device number from menu

make -j4 otapackage

# 14) Find the ROM:
out/target/product/ >> /romname.zip

Flash ROM & Enjoy

# Extra Commands

to cleanup before build

make installclean

to delete output and more

make clean

to delete output and all cache  (simillar to make clean)

make clobber

# Jack issues

Having jack issues (ran out of memory), follow steps below.

open a terminal on your desktop and put this into your terminal, 

substituting the # with how many GBs of RAM you have:

$ export JACK_SERVER_VM_ARGUMENTS="-Dfile.encoding=UTF-8 -XX:+TieredCompilation -Xmx#g"

now go into the root of the source folder and type the following:

./prebuilts/sdk/tools/jack-admin kill-server

 then

./prebuilts/sdk/tools/jack-admin start-server

now go back and do step 13) time mka nougat

********JAVA 1.8 INSTALL********

Add the repository and update apt-get:

sudo add-apt-repository ppa:webupd8team/java

sudo apt-get update

Install Java8 and set it as default:

sudo apt-get install oracle-java8-set-default

Check version:

java -version

******JAVA 8 REMOVE TO USE 7 FOR MM******

sudo apt-get purge oracle-java8-installer

Then type:

javac -version

The output must be:

javac: command not found

and to install java 7 in Ubuntu I use this code in terminal:

sudo add-apt-repository ppa:webupd8team/java

sudo apt-get update

sudo apt-get install oracle-java7-installer

After that type the following to see if there is java installed:

java -version

The output must be:

java version "1.7.0_80"


********INLINE KERNEL*********

go to build/core/tasks

open kernel file

## Internal variables

hash this one

#KERNEL_OUT := $(TARGET_OUT_INTERMEDIATES)/KERNEL_OBJ

add this one

KERNEL_OUT := $(ANDROID_BUILD_TOP)/$(TARGET_OUT_INTERMEDIATES)/KERNEL_OBJ

make sure this one is like this

KERNEL_CONFIG := $(KERNEL_OUT)/.config 

now go to device/moto/shamu

open BoardConfig.mk

# Inline kernel building

TARGET_KERNEL_CONFIG := six_defconfig < your config

TARGET_KERNEL_SOURCE := kernel/moto/shamu

BOARD_KERNEL_IMAGE_NAME := zImage-dtb

KERNEL_TOOLCHAIN_PREFIX := arm-eabi-

KERNEL_TOOLCHAIN := $(ANDROID_BUILD_TOP)/prebuilts/gcc/$(HOST_OS)-x86/arm/arm-eabi-4.8/bin

now grab the tool chain from source or uber

https://bitbucket.org/UBERTC/arm-eabi-4.8

now add the toolchain here

prebuilts/gcc/linux-x86/arm/arm-eabi-4.8

now add your kernel source here
 
kernel/moto/shamu/

to build just the kernel

make clean 

then 

make -j4 bootimage

