![](https://github.com/Manjil-sharma/Cisco-1814-Router-Recovery/blob/main/Cisco-1814/cisco-1814.jpg?raw=true)  


# Cisco-1814-Router-Recovery

In this repository, I will document the steps I took to recover a Cisco 1814 Router.<br>

## Introduction

When I started my Bachelor's program at college, I connected with the college's IT administrator, Mr. Subham Bohora, early on. During our conversation, he showed me some old devices that were no longer in use. Among them were a CISCO Switch and a CISCO-1814 Router, which, as a networking enthusiast and a fan of Cisco products, naturally piqued my interest. Luckily, Mr. Subham had a Console cable, which I used to try and access the Cisco Switch. I was able to access it easily, but I couldn't access the router no matter how many times I tried. Eventually, I gave up and forgot about it.<br>

However, my interest in the router was rekindled when I started my second semester. I wanted to see if I could fix it, so I attempted to access it again. To my delight, this time the router booted into ROMMON (ROM Monitor) mode, which allowed me to type some commands and see what was happening.<br>
<p align="center">
  <img src="https://github.com/Manjil-sharma/Cisco-1814-Router-Recovery/blob/main/Cisco-1814/1.PNG?raw=true"><br>
  <em>Accessing the router using PuTTY</em>
</p>

- NOTE<br>

      ROMMON, short for ROM Monitor, is a special diagnostic mode and bootstrap program used by Cisco devices, including routers 
      and switches.It is a low-level software that runs on the device's hardware and provides basic functionalities for booting 
      the device, loading the operating system, and troubleshooting various issues.
 <br>     
I didn't have much knowledge about the ROMMON environment. I had only read about it and knew only a few commands. Fortunately, Cisco has a helpful option available. With my limited knowledge and the help option, I was able to access and view the contents of the flash memory.<br>

<br>
<p align="center">
  <img src="https://github.com/Manjil-sharma/Cisco-1814-Router-Recovery/blob/main/Cisco-1814/2.PNG?raw=true">
</p><br>

<p style="color:red;"></p>

I made a casual attempt to boot the image, but encountered this error.

<br>
<p align="center">
  <img src="https://github.com/Manjil-sharma/Cisco-1814-Router-Recovery/blob/main/Cisco-1814/3.PNG?raw=true">
</p><br>

- After conducting some research, I discovered that the "**loadprog: bad file magic number: 0x0**" error message signifies that the program cannot be executed by the operating system due to its unexpected format. This error typically arises when attempting to launch a binary executable file that has either been corrupted or is not compatible with the system architecture.<br>

- The second error message, "**boot: cannot load flash:**", clearly indicates that there is an issue with the flash memory.

- These two error occur together and indicate that the system failed to load the specified file from the flash memory.<br>

Then I used **dir flash:** command to see the content of flash memory<br>

<br>
<p align="center">
  <img src="https://github.com/Manjil-sharma/Cisco-1814-Router-Recovery/blob/main/Cisco-1814/4.5.PNG?raw=true">
</p><br>


Based on the results, it is apparent that the IOS image is absent as it typically ends with the **".bin"** extension, which was not found.
This prompted me to conduct further research about the other files, their functions, and usefulness.<br>


## More on dir flash: result

I have not delved this deeply into Cisco before, but I have become interested and would like to explore further. To begin, I have previously discussed flash memory, but there are actually several types of memory utilized in Cisco devices. Cisco devices use several types of memory to perform their functions. Here are some of the main types of memory used in Cisco devices:<br>

- RAM (Random Access Memory): This is volatile memory that is used to temporarily store **running configurations**, routing tables, ARP caches, and other dynamic information. RAM loses its contents when the device is powered off or restarted.<br>

- ROM (Read-Only Memory): This is non-volatile memory that stores the device's firmware and basic operating system. ROM is read-only, so its contents cannot be modified.<br>

- NVRAM (Non-Volatile Random Access Memory): This is non-volatile memory that is used to store the device's **startup configuration**. NVRAM retains its contents even when the device is powered off or restarted.<br>

- Flash Memory: This is non-volatile memory that is used to store IOS images, configuration files, and other software components. Flash memory retains its contents even when the device is powered off or restarted.<br>

- EEPROM (Electrically Erasable Programmable Read-Only Memory): This is non-volatile memory that is used to store small amounts of configuration data, such as MAC addresses or serial numbers. EEPROM can be erased and reprogrammed electronically.<br>

Let's delve further into the outcomes of the 'dir flash:' command. After the program loaded successfully, with an entry point of 0x8000f000 and a size of 0xcb80, the following files were found in the flash directory:

<br>
<p align="center">
  <img src="https://github.com/Manjil-sharma/Cisco-1814-Router-Recovery/blob/main/Cisco-1814/4.PNG?raw=true">
</p><br>


The "**entry point**" refers to the memory address where the program starts executing after being loaded into memory. The specific memory address of the entry point is shown as "**0x8000f000**" in the output. In general, the entry point is the first instruction that is executed when a program is run. It typically contains code that initializes the program's data structures and sets up the program's execution environment.<br>

The "**size**" refers to the size of the program that was loaded into memory. The specific size of the program is shown as "**0xcb80**" in the output. The size of a program is typically measured in bytes and indicates the amount of memory that is required to store the program. In this case, the size of the program is 0xcb80 bytes, which is equivalent to 52160 bytes. This means that the program occupies a total of 52160 bytes of memory in the Cisco device's flash memory.<br>


We can now observe the contents of the flash memory, which comprise several files as displayed in the above figure. Looking at the figure, we can observe four columns that contain a combination of numbers and letters.<br>

### What is 5637?
In the context of the output for the "**dir flash:**" command, "**5637**" refers to the first column of the listed files, which displays the file's inode number or index node number. The inode is a data structure used by the file system to store information about a file, including its attributes and location on the storage device.<br>

In this case, the inode number "**5637**" represents the first file in the flash directory, which is "**sdmconfig-18xx.cfg**". Each file in the flash directory has a unique inode number assigned to it.

#### Index Node Number
An index node number, also known as an inode number, is a unique identifier assigned to each file and directory in a file system. Inodes are used by the file system to store metadata about a file or directory, such as its owner, permissions, creation and modification times, and the file's location on the storage device.<br>

The inode number is an integer value that is used by the file system to locate the file or directory on the storage device. It is a unique identifier for the file or directory, which allows the file system to distinguish it from other files or directories with the same name or path.<br>

### What is 2746?

In the output of the "dir flash:" command on Cisco devices, the second column of the listed files represents the size of the file "sdmconfig-18xx.cfg" in bytes.Each file in the flash directory has a specific size, which is determined by the amount of data it contains. The size of the file is important because it determines the amount of storage space it occupies in the flash memory.<br>


- Similarly the "**-rw-**" characters refer to the file's permissions and indicate the level of access that users have to the file. The first character indicates the file type, where "-" indicates a regular file, and "d" indicates a directory. The next three characters represent the file's permissions for the owner, group, and others. In "-rw-", the "r" stands for read permission, "w" stands for write permission, and the hyphen "-" represents that no execute permission is granted.<br>

- "**sdmconfig-18xx.cfg**" is a filename listed in the output of the "dir flash:" command.

####  sdmconfig-18xx.cfg

The sdmconfig-18xx.cfg file in a Cisco 1841 router flash stores the configuration settings for the SDM (Security Device Manager), which is the web-based graphical interface used to manage the router's security features. The file contains the SDM configuration settings, such as the access control list (ACL) entries, firewall policies, and VPN configurations.<br>

#### es.tar

The es.tar file in a Cisco device is typically a backup file for the device's IOS (Internetwork Operating System) software. It contains the image of the device's operating system, which can be used to restore the system to a previous state in the event of a software failure or to upgrade the system to a newer version. The ".tar" extension indicates that the file is a tar archive, a type of compressed file format commonly used for software distribution.<br>

#### common.tar

The common.tar file in a Cisco device is a backup file that contains configuration information, such as device settings and configuration files, that are common across multiple devices. This information can be used to quickly configure a new device or to restore an existing device to a previous state in the event of a failure.<br>

#### home.shtml

The home.shtml file in a Cisco device is a web page file, typically used for the device's web interface. The ".shtml" extension indicates that the file contains both HTML (Hypertext Markup Language) code and server-side scripting code, such as Perl or CGI (Common Gateway Interface) scripts. This file is used to display the device's status and configuration information to users via a web browser. It is also used to provide a graphical user interface for configuring and managing the device.<br>

#### home.tar

The home.tar file in a Cisco device is a backup file that contains the configuration information and web interface files for the device's web server. This file can be used to restore the device's web server to a previous state in the event of a failure or to transfer the web server configuration to a different device.<br>


## TFTP Server Setup and OS Recovery process

Upon conducting some research, I discovered that in order to transfer the OS file to the router, I needed to create a TFTP server. Consequently, I installed Tftpd64 and began my search for the iOS image.<br>

<br>
<p align="center">
  <img src="https://github.com/Manjil-sharma/Cisco-1814-Router-Recovery/blob/main/Cisco-1814/tftp1.PNG?raw=true">
</p><br>

After conducting further research on Cisco's official sites and the Cisco Learning Community, I discovered that Cisco announced the end of life announcement for the Cisco 1814 in 2014, and the end of maintenance date was in 2015. As a result, it is no longer possible to locate the OS for the Cisco 1814 on Cisco's official website.<br>

I searched through various unofficial sites, but was unable to find the exact operating system for my 1814 router. Eventually, I decided to use **c1841-adventerprisek9-mz.124-9.t1.bin**, which was a genuine OS provided to me by a friend. Although I was initially hesitant about its compatibility, I decided to proceed with the installation.<br>












      

