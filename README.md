# android-studio-setup-ubuntu
:ballot_box_with_check: [Setup Guide] Android Studio Setup Guide in Ubuntu 16.04

This is a working guide to get Android Studio up and runnning on Ubuntu. Android Studio is provided for free by Google and powered by the IntelliJ Platform. Use of Eclipse for Android development has since been deprecated.

## TOC
* [Install Java](#install-java)  
* [Install Android Studio](#install-android-studio)  
* [Install the SDKs](#install-the-sdks)
* [Add your Android Studio Project](#add-your-android-studio-project)
* [Install Intel's KVM for Better AVD Performance](#install-intels-kvm-for-better-avd-performance)

## Install Java
1. Android Studio requires JRE 6 and Oracle JDK 7 (not OpenJDK) to function properly. Absence of JRE 6 will prevent it from starting. Absence of Oracle JDK 7 will prompt stability warnings.

2. Check the version(s) of Java you have.

    ```
    update-java-alternatives -l
    ```

    If the result contains these:

    ```
    java-1.6.0-openjdk-amd64 1061 /usr/lib/jvm/java-1.6.0-openjdk-amd64
    java-7-oracle 1072 /usr/lib/jvm/java-7-oracle
    ```

    you have what you need and you can skip to the next section. Else, continue to install Java

3. Update the package index.

    ```
    sudo apt-get update
    ```

4. Install JDK 6.

    ```
    sudo apt-get install openjdk-6-jdk-headless
    ```

5. Install Oracle JDK 7. The Oracle JDK is the official JDK; however, it is no longer provided by Oracle as a default installation for Ubuntu.

    ```
    sudo apt-get install python-software-properties
    sudo add-apt-repository ppa:webupd8team/java
    sudo apt-get update
    sudo apt-get install oracle-java7-installer
    ```

6. Check that you have everything with

    ```
    update-java-alternatives -l
    ```

    and you should see at least:

    ```
    java-1.6.0-openjdk-amd64 1061 /usr/lib/jvm/java-1.6.0-openjdk-amd64
    java-7-oracle 1072 /usr/lib/jvm/java-7-oracle
    ```

## Install Android Studio
You have **two** options.

#### (1) Getting the package from Google
1. Go to the [Android Developers site](http://developer.android.com/sdk/index.html) and download the full package (not just the SDK) into a directory of your choosing.

2. Unpack/Extract the ZIP file into the selected directory. Navigate to android-studio/bin/ in Terminal and run Android Studio

    ```
    ./studio.sh
    ```

#### (2) Installing with apt-get
Provided by [Paolo Rotolo](http://paolorotolo.github.io/android-studio/)  
*Student Developers should consult their supervisor before adding third-party repositories*

    ```
    sudo apt-add-repository ppa:paolorotolo/android-studio 
    sudo apt-get update
    sudo apt-get install android-studio
    ```

If no prompt pops up, go to "usr/share/applications" to find the .desktop and run the app. Right click on the icon on the desktop sidebar and "Lock to Launcher".

#### First-time Prompts
* Complete Installation **I do not have..** *Unless you do.*
* Android Setup Wizard
    * Install Type **Standard**
    * Emulator Settings (We'll deal with this in the next steps)
    * License Agreement **Accept android-sdk-license and Finish**  
    Your *Sdk* folder will be in "home/Android"

#### Update your Android Studio
1. Go to Configure > Settings > Updates **OR** File > Settings > Updates
2. Check "Check for updates in channel"
3. Set the channel to Stable Channel unless you're feeling adventurous.
4. Check Now

## Install the SDKs
1. Go to Configure > SDK Manager **OR** Tools > Android > SDK Manager
2. Install these packages:  
_Revisions are latest at the time of writing. Let's just say you'll take the latest of these or something similar._
    
    * Tools
        * Android SDK Tools [Rev 24.1.2]
        * Android SDK Tools Platform-tools [Rev 22]
        * Android SDK Build-tools [Rev 21.1.2]
        * Android SDK Build-tools [Rev 19.1]
        * Android SDK Build-tools [Rev 19.0.3]
    * Android 5.1
        * Documentation for Android SDK [API 22, Rev 1] --_useful to have_
        * SDK Platform [API 22, Rev 1]
        * Google APIs [API 22]
        * Google APIs Intel x86 Atom_64 System Image [API 22, Rev 1]
        * Google APIs Intel x86 Atom System Image [API 22, Rev 1]
    * Android 5.0.1 (API 21)
        * SDK Platform [API 21, Rev 2]
        * Google APIs [API 21, Rev 1]
        * Google APIs Intel x86 Atom_64 System Image [API 21, Rev 4]
        * Google APIs Intel x86 Atom System Image [API 21, Rev 4]
    * Android 4.4.2 (API 19) --_for Android KitKat minimum support_
        * SDK Platform [API 19, Rev 4]
        * Google APIs (x86 System Image)
    * Android 4.1.2 (API 16) --_for Android JellyBean minimum support_
        * SDK Platform [API 16, Rev 5]
        * Google APIs [API 16, Rev 3]
    * Extras
        * Android Support Repository [Rev 12]
        * Android Support Library [Rev 22]
        * Google Play Services [Rev 23]
        * Google Repository [Rev 16]

    _Accept the agreements by selecting the top level paths._

## Install Intel's KVM for Better AVD Performance
The Android Virtual Device (AVD) lets you test the app on the computer instead of using your own device. **8GB of RAM** seems to work well. This setup guide (based on [Intel's](https://software.intel.com/en-us/blogs/2012/03/12/how-to-start-intel-hardware-assisted-virtualization-hypervisor-on-linux-to-speed-up-intel-android-x86-emulator)) is mainly to make the AVD work fast and smooth using hardware virtualization.

1. Check if your processor supports hardware virtualization in Terminal:

    ```
    egrep -c '(vmx|svm)' /proc/cpuinfo
    ```
    Output **0** means no (sadface). If this is the case, you may prefer to use an actual Android device for testing. And you can skip the next steps of AVD.

2. Install cpu-checker

    ```
    sudo apt-get install cpu-checker
    ```

3. Check if your cpu supports kvm

    ```
    kvm-ok
    ```
    If you see:
    ```
    INFO: Your CPU supports KVM extensions
    INFO: /dev/kvm exists
    KVM acceleration can be used
    ```
    it means you can run your virtual machine faster with the KVM extensions. (Yeay)

    But if you see
    ```
    INFO: KVM is disabled by your BIOS
    HINT: Enter your BIOS setup and enable Virtualization Technology (VT),
    and then hard poweroff/poweron your system
    KVM acceleration can NOT be used
    ```
    then you need to go to the BIOS Setup and enable VT.

4. Enabling VT (done with HP Compaq 8200)
    1. Restart the computer.
    2. Enter the BIOS Setup (Computer Setup in HP) by spamming F10 on startup.
    3. In Setup, go to Security > System Security.
    4. Enable these:
        ```
        Virtualization Technology (VTx)
        Virtualization Technology Directed I/O (VTd)
        ```
    5. Save changes and exit.

5. Install the KVM
    
    ```
    sudo apt-get install qemu-kvm libvirt-bin ubuntu-vm-builder bridge-utils
    ```
    You may ignore the Postfix Configuration prompt by selecting "No Configuration".

6. Add your local user account to the group kvm and libvirtd.

    ```
    sudo adduser your_user_name kvm
    sudo adduser your_user_name libvirtd
    ```
    After the installation, you need to relogin so that your user account becomes an effective member of kvm and libvirtd user groups. The members of this group can run virtual machines.

7. Verify installation in Terminal:

    ```
    sudo virsh -c qemu:///system list
    ```
    If you see:
    ```
    Id Name                 State
    ----------------------------------
    ```
    installation was a **success**! Just a little bit more.

8. Add the emulator command line options.  
    1. In Android Studio, go to Run > Edit Configurations.
    2. To set as global default for all projects, select "Android Application" under "Defaults".
    3. Go to the emulator tab, check "Additional command line options" and add:
        
        ```
        -qemu -m 2047 -enable-kvm
        ```

## Create Your AVD
1. In Android Studio, go to Tools > Android > AVD Manager.

2. Click on "Create Virtual Device".

3. Choose a Phone (Nexus devices are pretty standard).

4. Choose a System Image. Lollipop (API 22) is the latest at the time of typing. The target should be "Google APIs (Google Inc.).." for our app to function with Google Maps.

5. In AVD (Verify Configuration),
    * Check "Use Host GPU".
    * Show Advanced Settings and make sure RAM is more than 1024 (but max at 1024 if your OS is 32-bit).

6. Finish. This shouldn't take too long..

7. Start up your AVD using Terminal. Navigate to your Android "Sdk/tools" folder (usually in ~/Android). Put in the path to your "Sdk/tools/lib" folder for "LD_LIBRARY_PATH" below: 
    
    ```
    LD_LIBRARY_PATH=path/to/sdk/tools/lib64 ./emulator64-x86 -avd Nexus_5_API_21 -qemu -m 2047 -enable-kvm
    ```

    _For those with a 32-Bit system, modify the command to below:_
    ```
     LD_LIBRARY_PATH=path/to/sdk/tools/lib ./emulatorx86 -avd Nexus_5_API_21 -qemu -m 1024 -enable-kvm
    ```
    "Nexus_5_API_21" is a sample name for the AVD created. Replace it with your AVD's name replacing spaces with underscores. 
    
    It shouldn't take more than a minute to start up. If the screen goes all gibberish, just swipe up (Android Lollipop unlock gesture) and the home screen should appear before you.
