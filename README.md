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
  <img src="https://github.com/Manjil-sharma/Cisco-1814-Router-Recovery/blob/main/Cisco-1814/4.PNG?raw=true">
</p><br>


Based on the results, it is apparent that the IOS image is absent as it typically ends with the **".bin"** extension, which was not found.
This prompted me to conduct further research about the other files, their functions, and usefulness.<br>


## Research And OS Recovery process


I have not delved this deeply into Cisco before, but I have become interested and would like to explore further. To begin, I have previously discussed flash memory, but there are actually several types of memory utilized in Cisco devices. Cisco devices use several types of memory to perform their functions. Here are some of the main types of memory used in Cisco devices:<br>

- RAM (Random Access Memory): This is volatile memory that is used to temporarily store **running configurations**, routing tables, ARP caches, and other dynamic information. RAM loses its contents when the device is powered off or restarted.<br>

- ROM (Read-Only Memory): This is non-volatile memory that stores the device's firmware and basic operating system. ROM is read-only, so its contents cannot be modified.<br>

- NVRAM (Non-Volatile Random Access Memory): This is non-volatile memory that is used to store the device's **startup configuration**. NVRAM retains its contents even when the device is powered off or restarted.<br>

- Flash Memory: This is non-volatile memory that is used to store IOS images, configuration files, and other software components. Flash memory retains its contents even when the device is powered off or restarted.<br>

- EEPROM (Electrically Erasable Programmable Read-Only Memory): This is non-volatile memory that is used to store small amounts of configuration data, such as MAC addresses or serial numbers. EEPROM can be erased and reprogrammed electronically.<br>

Let's delve further into the outcomes of the 'dir flash:' command.<br>







      

