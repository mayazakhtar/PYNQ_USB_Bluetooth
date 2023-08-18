# PYNQ_USB_Bluetooth
In this Git we are going to set bluetooth device for Pynq-Z1 board.
**Software:** Vivado 2019.1, Petalinux 2019.1, Serial Terminal. I'm using Ubuntu 18.04.6 on VMware.                            
**Hardware:** PYNQ-Z1 board, USB Bluetooth Dongle (CSR 4.0)                                                  

Steps:
1. First Download Pynq-Z1 git repo by using following command **git clone --recurse-submodule https://github.com/Xilinx/PYNQ.git**
2. Now change this repo for 2.5 version by this command **git checkout origin/image_v2.5**
3. Change directory to sdbuilds **cd <pynq_dir>/sdbuild**
4. Run this command **source ./scripts/setup_host.sh**
5. Source vivado, SDK and petalinux.
6. run the make file in sdbuild directory to create petalinux project. **make BOARDDIR=<pynq_dir>/boards**
7. When everything is done go to <pynq>/sdbuild/build/Pynq-Z1/petalinux_project
8. Run **petalinux-config -c kernel** for kernel configuration Networking support ---> Bluetooth Subsystem support                      
       --- Bluetooth subsystem support                                    
      **[*]**   Bluetooth Classic (BR/EDR) features                         
      **<*>**     RFCOMM protocol support                                   
      **[*]**       RFCOMM TTY support                                      
      **<*>**     BNEP protocol support                                    
      **[*]**       Multicast filter support                               
      **[*]**       Protocol filter support                                
      **<*>**     HIDP protocol support                                    
      **[*]**     Bluetooth High Speed (HS) features                       
      **[*]**   Bluetooth Low Energy (LE) features                         
      **[ ]**   Enable LED triggers                                        
      **[ ]**   Bluetooth self testing support                             
      **[*]**   Export Bluetooth internals in debugfs                      
            Bluetooth device drivers  --->
9. In Bluetooth device drivers. Enable HCI USB driver.                             
   **<*>** HCI USB driver                             
   **[ ]**   Enable USB autosuspend for Bluetooth USB devices by default                             
   **[*]**   Broadcom protocol support                             
   **[*]**   Realtek protocol support
10. Save and Exit. Run **petalinux-build** Create BOOT.BIN and boot the board.
11. After booting run the following commands.
    1. **sudo apt-get update**
    2. **sudo apt-get install bluez**
    3. **sudo service bluetooth start**
    4. **sudo service bluetooth status**
    5. **bluetoothctl**
    6. **power on**
    7. **discoverable on**
    8. **scan on**
    9. **pair mac_address**
    10. **quit**
12. We are going to use bluedot for test. First install bluedot
    1. **sudo apt install python3-dbus**
    2. **sudo pip3 install bluedot**
    3. Also install Bluedot app on your Android mobile.
13. Open python and run the following code for bluedot  
    from bluedot import BlueDot  
    bd = BlueDot()  
    while True:  
    &emsp;bd.wait_for_process()  
    &emsp;print("led on")  
    &emsp;bd.wait_for_release()  
    &emsp;print("led off")
14. Open android app and connect it to your bluetooth device. After connection bluedot will appear on app. Touch this dot to send bluetooth signal.
