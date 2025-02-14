---
hide:
  - navigation
---

<style>
    div.admonition p:not(.admonition-title) {
        font-size: 125%;
    }
    .center {
        display: block;
        margin-left: auto;
        margin-right: auto;
    }
</style>

# How to install ReviOS

In this guide you will see how to install ReviOS (or any Windows for that matter).

We discuss here the installation of ReviOS with a **USB drive** (or usually called a flash drive in the guide). This guide also mentions a way to boot into the installer without a USB drive, but the main method is to use a flash drive. Another way is to use the script of iidanL, [InstallWindowsWithoutUSB](https://github.com/iidanL/InstallWindowsWithoutUSB){target=_blank}. We do not support this method of installing, use it at your own risk. (TLDR: It sets up your new Windows to dual boot.)

## Step 0: Preparations

### BIOS vs UEFI, MBR vs GPT

First, if you do not know if your PC is capable of UEFI booting, please consult your motherboard's or laptop's manual. 

Or go into your BIOS, you can do that ususally by spamming the ++f2++ or ++delete++ button when your PC is starting up. If it has a graphical interface that you can use with your mouse and not *hurting your eyes to look at*, you most likely have UEFI. In every other scenario, you probably have BIOS.

Just to be sure, inside the booting menu, and search for a setting where you can switch between option usually called "UEFI" or "Extended" and "Legacy" or "BIOS".

These technologies usually come in pair, like UEFI and GPT or BIOS and MBR. And while you could switch things up, like UEFI and MBR or BIOS and GPT, it is not recommended. Here, for example, the UEFI and GPT pair means UEFI boot mode on GPT partitions. Also, UEFI systems usually backward compatible and can boot in Legacy mode, often called CSM. If your system have CSM, it is recommended to switch it off, because the booting probably will be done in Legacy or BIOS mode otherwise.

!!! tldr
    In short, if your PC is **capable of** booting in **UEFI** mode, we recommend **selecting GPT**, if not, select MBR later on in the step [Making a bootable flash drive](#step-2-making-a-bootable-flash-drive). 

### Drivers

The second recommended thing to do before doing any other thing, is to download your drivers, at least your **network drivers**. It is much easier to just download the driver before reinstalling, rather than booting into your new installation of ReviOS, and realizing you have no internet, and to download your driver for internet, you need internet.

If you do not know how to download your driver, we can give you a general idea, but the manufacturer's websites are not the same. 

The fastest way to find the drivers you need, just **Google** "<*the name of your motherboard or laptop*> drivers", of course without the `<` and `>` symbols. Click the search result which takes you to the **website of the manufacturer** of your device. There find the drivers, probably under the **Support section** or something like that.

After you accuired your drivers, should take them with yourself on the installation **flash drive**. So you need to **copy** the files **after the** [**2nd step**](#step-2-making-a-bootable-flash-drive), but before the [3rd](#step-3-booting-the-flash-drive)

Alternatively, and ==this method should not be considered the default option==, download the network drivers with Snappy Driver Installer Origin and take the whole folder of it with you before you format your partition(s). Help on this method can be found on the [How to install drivers page](drivers.md#a-last-resort).

### Check what drives and partitions you have

The third thing recommended thing before installing, is to check what drives and partitions you have. More specifically, how are they identified.

!!! help "Explanation"
    Before getting into that, let me explain how this whole drives and partitions thing works.
    
    You have in your PC one or more drives. Those can be SSD or HDD, but for this explaination does not matter.
    
    And on one drive you have one or more partitions. This guide mostly talks about partitions.

    !!! note
        Windows uses the terms "disk" and "drive" interchangeably. So they mean the same.


Open `Disk Management` by right-clicking on the Start menu icon, and selecting it.

Should look like this:

![disk_management](img/install/0_disk_management.png){.center}

The **recommended** way to do the installation is to at least **format** (or more like delete and recreate, you will see it at [Installing](#step-4-installing)) the **partitions storing the OS**. Usually the partition that was called C: by the Windows on it, and other **System** and **Reserved** and **Recovery** partitions. Additionally, you can format your other partitions too, but **make sure you backed up everything**.

!!! tip
    Additional advice is if you are worried you could not identify your partitions inside the installer, take note of the sizes and the number of them. If you have multiple partitions with the same size, take note of the free space, too.

    !!! example
        Take for an example the picture above.

        Here I have two drives, with the first having 3 partitions, and the second only one. This is a common setup, where the user has a drive which holds the OS, and another drive for everything else, e.g. data, movies, games.

        I would note the following:
        
            Need to be deleted:
            - System Reserved partition, Disk 0 Partion 1, size: 50 MB
            - the main partition of Windows (C:), Disk 0 Partion 2, size: 49.48 GB
            - Recovery partion, Disk 0 Partion 3, size: 475 MB

            Free to do whatever you want:
            - data partition, Disk 1 Partition 1, size: 100 GB
        

        ---

        Another example would be if you have similar like on the picture but basically the D: 100 GB partition is on the first disk, not on the second. Here again I would note almost the same, execept the data partition is Disk 0, not 1; and Partition 4, not 1.


!!! tldr 
    To reiterate, you will need to delete all OS partitions, not just the C: partition, but every system, reserved, recovery, etc. partitions too. These you can easily identify by the small, an only couple of megabytes size of them.

### Backup

!!! danger
    This Windows installing process will delete your previous OS, so you should back up everything you plan on seeing again in the future. At least have separate partition to save your data. Or you can move your things to a flash drive, even the installing flash drive, just after the [2nd step](#step-2-making-a-bootable-flash-drive), but before the [3rd](#step-3-booting-the-flash-drive)


## Step 1: Download ReviOS

You can find our supported versions on the [Download page of the Revision website](https://www.revi.cc/revios/download){target=_blank}.

Here download the the `Clean Installation` option of the version of your choice. Help about choosing the right ReviOS: [What is the difference between the versions?](related.md#what-is-the-difference-between-the-versions)

### Verification

!!! attention
    This step is very important, it can save you a lot of time in the future.

=== "Automatic Method"

    Using an automatic tool made by [Stasium](https://github.com/StasiumDev/), you can automagically verify the downloaded ISO. You can download it [here](https://github.com/StasiumDev/ReviOS-Verifier/releases). 
  
    After downloading the .zip file, extract it to a location of your choice. Here you will see a `revi-verifier.exe` file. Drag the downloaded Revi ISO onto this program. It will scan the file and check if it has downloaded successfully. 
  
    If the program reports an error with the file, you should download the ISO again, preferrably from another source. Multiple alternative sources can be found on the [Revision Discord](https://discord.gg/962y4pU).

=== "Manual Method"

    On Windows, go in the File Explorer where you downloaded the ISO file, and ++shift+"Right-click"++ on an empty space of the window, and select `Open PowerShell window here`. Type in the following command: 

    ```powershell
    Get-FileHash -Algorithm MD5 -Path <file name>
    ```

    And replace the `<file name>` part with the name of the ReviOS installer file's name. **You can use the ++tab++ button to autocomplete the file name.** For example: `Get-FileHash -Algorithm MD5 -Path .\ReviOS-10-22.10.iso`

    When the command returns with the hash of the file, compare it to the corresponding MD5 hash value on the [Verification page on the Revision website](https://www.revi.cc/revios/download/verify){target=_blank}. If they are the same, your ISO file is not corrupted, you can carry on with the process.

## Step 2: Making a bootable flash drive

### Ventoy

<!--What makes Ventoy a better choice over Rufus is that you can use it on Linux too, and it's much easier.-->

!!! danger
    This method will **wipe everything** from your flash drive. If you want to see the contents of it in the future, ==**make a backup**==. You can copy back the contents of the flash driver after Ventoy did it's thing. The partition probably will be named `Ventoy`.

!!! tldr "Tldr: on how Ventoy works"
    With Ventoy, you don't need to format the flash drive over and over, you just need to copy the image files to the USB drive and boot them directly. You can copy many files at a time and ventoy will give you a boot menu to select them.
    And actually you will boot a hidden partition, with the sufficient software to boot up an image file after you select it.

You can download Ventoy on the [official website of the software](https://www.ventoy.net/en/download.html){target=_blank}.

You do not need to install Ventoy, after extracting the zip file, just open `Ventoy2Disk.exe`.

Should look something like this:

![ventoy1](img/install/1_ventoy1.png){.center}

**Select** your **USB device** from the **dropdown menu**. If it is not present, in the `Option` menu select the `Show All Devices` option to show everything. But be sure to select the right device in the dropdown menu, because with this enabled, every storage device will be listed!

Select the partition style/scheme of your choice from **Options**, **Partition Style**. More detail in [BIOS vs UEFI, MBR vs GPT section](#bios-vs-uefi-mbr-vs-gpt)

![ventoy2](img/install/2_ventoy2.png){.center}

Additionally, you can select the `Secure Boot Support`, especially if you have Windows 11 and plan on playing with games or anti-cheat that require it.

Now click on the `Install` button.

After the software finishes, you can close the window.

You can now **copy** the ReviOS installer **ISO** file to the partition of the **flash drive**, called `Ventoy`.

On Linux, the method of using Ventoy should be the same.

<!--
### Rufus

You can download Rufus [here](https://rufus.ie/){target=_blank}.

Like Ventoy, this software also does not need installation. Should look like this, after opening the EXE file:

![rufus1](img/install/3_rufus1.png)

Here, first select your flash drive under the `Device` dropdown menu. If it is not present, click `Show advanced drive properties`, and tick `List USB Hard Drives`.

Then select the ISO file by clicking on the `SELECT` button.

With ReviOS 10 the only thing you need to do before starting to prepare your flash drive is to decide MBR or GPT you will use. If you do not what are those, this guide wrote in detail about it [above](#bios-vs-uefi-mbr-vs-gpt).

Should look something like this with ReviOS 10:

![rufus_win10](img/install/4_rufus_win10.png)

With ReviOS 11 select GPT. There is one more thing to look out for, and make sure you do not change it, no matter what hardware you have. It is the `Image option`. Again, it does not matter that you have TPM or not, ==**do not change this option from `Standard` to `Extended`**==, because the ReviOS installer will not work!

Should look something like this with ReviOS 11:

![rufus_win11](img/install/5_rufus_win11.png)

Now you can start the preparation, by clicking the `START` button
-->

### If you do not have a USB drive

...on Windows you can go into Disk Management, select one of your partitions that have atleast 4 to 5 gigabytes of free space. ++"Right-click"++ on that partition, and select `Shrink Volume...`. Then a window asks how many space do you want to cut of that partition, in megabytes. Write in 5120, but at least the size of the ISO file times 1024 plus 100.

A new partition will pop up. ++"Right-click"++ on it, and select `New Simple Volume...`. A setup wizard will appear, there you can select the letter and name of the partition, although those have not much relevance.

If you are done, the formatted partition will be ready for you to extract the contents of the ISO file. If you went with this method, the following steps should be the same, just this guide will call your partition with the installer's file a flash drive, not a partition.

## Step 3: Booting the flash drive

<!-- To boot into your flash drive, open `Start menu`, click on `Power`, and then hold ++shift++ and click on `Restart`. This way, a full screen menu should come up looking like this:

![6_shift_restart](img/install/6_shift_restart.png)

There if you have the option `Use a device`, click it, and then the option with the name of your flash drive or `EFI USB Device`. -->

### Boot menu

First **consult the manual** of your motherboard or laptop, and find out **which key** is the **Boot Menu**.

When you restart your PC, after it shutdown, and just started to boot up again, start **spamming that key** until the menu opens.

A list should come up with options to boot. Also, usually the last option is to enter BIOS. Here, choose the option with the **name of your flash drive**. If you have multiple options with the name of it, choose that boots in UEFI mode, if you selected GPT when preparing the flash drive. Or if it does not show the names, only USB Drive and Optical Drive and Hard Drive, something like those, obviously choose the USB Drive like option.

### Boot order changing in BIOS

Alternatively you can go inside your BIOS settings, and change the boot order. This is a list that tells the BIOS what to boot. So when your PC boots, BIOS trys to boot from the first option from the list. If that is not working, it tries the second option, ans so on.

!!! note
    This option is not optimal, because if you are done with your Windows installation, but before booting the OS the first time, you have to change the boot order back in BIOS or remove the installer media, the flash drive, because you will boot into the installer again.


## Step 4: Installing

If you have done everything good so far, and used Ventoy, a menu will be open with the option to select and image to run. It will look similar to this image:

<figure markdown>
  ![Ventoy_menu](img/install/6.5_Ventoy_menu.png){.center width=50%}
  <figcaption markdown>Click on the picture to open it full size<br>Source: [Ventoy website](https://www.ventoy.net/en/screenshot.html)</figcaption>
</figure>

Select the desired image file. If you used Ventoy the first time, only one entry will be in the list.

When the installer boots up, it should look something like this:

![installer1](img/install/7_installer1.png){.center width=600px}

Accept the license terms, and then the disk selection step comes up. Here you have multiple choices, depending on the configuration you are using. 

### Partitions

!!! info "Missing driver"
    When you arrive to the partition making and selecting part of the installing process, but the installer asks for drivers, you might need Intel Rapid Storage Technology (IRST) drivers. [Here is a helping guide from the Asus Support page](https://www.asus.com/support/FAQ/1044458/).

!!! warning "Important"
    This part of the setup relies on the knowledge of your drives, which we discuss at [Check what drives and partitions you have section](#check-what-drives-and-partitions-you-have).


![installer2](img/install/8_installer2.png){.center width=600px}

Check what partitions your previous Windows installation uses, what you noted down, and delete those with the `Delete` button.

And then select the drive which used to store your previous Windows, and click `New`, write in the desired size of the new "C: partition" and add to that number 525 megabytes. If you are done, click `Apply`. 

If you did not change the number, it will create a new partition from the whole drive, and could and probably will make other, OS related partitions, as the pop-up window says: `To ensure that all Windows features work correctly, Windows might create additional partitions for system files.`

Additionally, if you want a clean start, you can format your other partitions too.

After you created the partitions you wanted, select the first you created, the one you meant for the OS, and click `Next`.

The installation started, you do not need to do anything for a while.

If the installer finishes, it will restart automatically.

Then the OS will load for a bit, with the text `Getting ready` on the screen.

Then it will restart again.

## Step 5: Windows post install setup

!!! note
    On Windows 11 this section looks different but, the steps should be the same. Furthermore, in newer builds of ReviOS, we added the possibility to change the language and keyboard here. This tutorial does not reflect that, since the pictures has not been updated in a while.

After the last restart you should arrive at this screen, obviously here you have to choose a username:

![post_install_username](img/install/9_post_install_username.png){.center width=600px}

And here a password. You can skip this question by just clicking on `Next`. 

!!! warning
    Skipping this can lead to a bug in ReviOS. More details [here](archive.md#i-cannot-log-into-my-user).

![post_install_password](img/install/10_post_install_password.png){.center width=600px}

Here just press `Not now`. Even if you press `Accept`, Cortana probably will not work.

![post_install_cortana](img/install/11_post_install_cortana.png){.center width=600px}

You are done with the Windows post install setup, the OS will load for some time.

## Step 6: Finishing the installation

After arriving at your desktop, first, **let the script that popped up run through**. **Do not do anything until the script restarts your PC**. If it is not closing, read [the section about this phenomenon](not-related.md#the-do-not-close-the-window-otherwise-your-system-wont-work-properly-window-not-closing).

After your PC boots up again, this guide have some recommendation to go through.

1. Activate your Windows with a valid Pro key. Help in the [ReviOS Post-Install guide on the Revision website](https://www.revi.cc/revios/post-install#h.p_wN71uiu-sFys){target=_blank}.

2. In `Settings` → `Time & Language` → `Language`, and add your language, even if you want to use Windows on English. To change your keyboard layout to non-English, you need to add that language. After that, you can change your regional format or keyboard layout or even what language Windows displays:
    
    ![language](img/install/12_language.png){.center}

3. Set the time, because ReviOS's default is the UTC time zone. `Settings` → `Time & Language` → `Date & time`

4. Download and install your drivers. This guide discussed downloading and installing drivers before, in great detail, check it [here](#drivers), and you can find more help on the [How to install drivers page](drivers.md).

5. Go through the [ReviOS Post-Install guide on the Revision website](https://www.revi.cc/revios/post-install){target=_blank} guide. You do not need to do every step, you can skip any of them, but the Visual C++ Runtimes and DirectX installation is highly recommended.

