## 🚀 Welcome to Week2 
<br>

## 💡 Today we can see how to install operating system on each nodes by configuring each steps and also configuring SSH.

----
> #### Step 1 (Prepare installation media USB by using personal device.)
 1. Download Ubuntu ISO [Image](https://releases.ubuntu.com/jammy/) (ubuntu LTS 22.04 jammy edition).

 2. Also install ISO flashing Software [balena_Etcher](https://etcher.balena.io/).
 
  3. Prepare USB drive of 16gb to 32gb.

  4. Open belena Etcher 
  
  ![Image](https://github.com/user-attachments/assets/78df882e-c7f4-4b9b-a907-2aa5a00ac5c6) 

   - Plug USB to device.
   - Select ISO image > ubuntuLTS22.04.iso
   - Select Target > Plugged_USB_Drive
   - Click Flash 🚀

   5. It will take some time to flash the ISO image to USB drive.

   6. This USB drive we use to install Operating system on each nodes.

   7. Whenever flashing is completed Unplug the USB drive.
   
----
   > #### Step 2 (configuring BIOS.)
   1. Step by step configuring each node's BIOS.
   2. POWER on 1'st node.
   3. By pressing POWER on key instantly press F2 + F1 + Esc and press Delete kay in fluctuation.
   4. If it opens BIOS screen it is good.
   5. If not then just Google the bios kay [hear](https://techofide.com/blogs/boot-menu-option-keys-for-all-computers-and-laptops-updated-list-2021-techofide/)
   
   6. If you find. it throws screen like 

   ![Image](https://github.com/user-attachments/assets/ca540436-f3ab-4b02-8355-113285c664ac)

   
   7. Go to Security Tab and disable the Secure boot.
   - Security Tab > Boot Menu/option > Disable.
   - Save Settings and Exit.

   ![Image](https://github.com/user-attachments/assets/fccd4b0b-ed57-4c09-877c-555a319138a2)
   
   
   8. Then POWER Off the node and Plug the installation media and then Restart the node then instantly go to BIOS. 

   9. Go to Boot tab And set Installation media as first priority. 
   - There are all connected storage list.
   - Find installation media and get in to first position.
   - use ***Space Bar to get the installation media on first position.***
   10. Then Just Reboot it. 
   11. It Displays the installation screen of ubuntu.

----
   > #### Step 3 (Installing Ubuntu.)
   1. Power on the node and it displays the Boot loader.
   2. Press enter on "Try or install ubuntu".

   ![Image](https://github.com/user-attachments/assets/a90d1588-a3f0-460d-b726-e083383c0933)


   3. Now it opens the Ubuntu installation Screen.
   - Select Language > English
   - Press on > Install ubuntu

   ![Image](https://github.com/user-attachments/assets/bd0e670f-6972-4fd1-af18-fa26e7d398c8)

   4. Now configure keyboard layout.
   - select > English US
   - Press > continue 

   ![Image](https://github.com/user-attachments/assets/190af531-2404-4ffb-9573-8eb5955a97d0)

   5. Updates and Other Software.
   - select > Normal installation 
   - Press > continue 

   ![Image](https://github.com/user-attachments/assets/be072e38-fda7-4f8b-b1bc-9f258256a6b8)

   6. Installation Type.
   - Select > Erase Disk and install ubuntu 
   - Press > continue 

   ![Image](https://github.com/user-attachments/assets/be072e38-fda7-4f8b-b1bc-9f258256a6b8)

   7. **Then It asks For partition Just select Recommended option.** 

   8. Select > Install Now > continue.

   ![Image](https://github.com/user-attachments/assets/3c4e0e89-4065-4810-b16d-70dd0adb25f7)


   9. Then it asks for Location/Time zone 

   - Select Time zone as per you.
   - Or just click on MAP.
   - Press > continue

   ![Image](https://github.com/user-attachments/assets/91c1495b-7ce2-4cfd-9013-998ccb65d4f4)

   10. Now it asks for User Info
   - Your name 
   - Username
   - Password
   - `Select > Log in automatically`
   - Press > continue 

   ![Image](https://github.com/user-attachments/assets/be4ec44e-c917-4ce4-aad7-18cefc2f6ca4)

   11. Now it can start Installing.

   ![Image](https://github.com/user-attachments/assets/e26494f3-4eb2-4d4e-b2f0-888585108cf2)

   12. Whenever the installation is completed is Displays this massage.
   - Press > Restart Now 
   - When the Black Screen Appears instantly unplug the installation media 
   - It automatically open Ubuntu as Os of node.

   ![Image](https://github.com/user-attachments/assets/8301aa6e-56d3-4b52-be35-d1246584dcb1)

----
   > #### Step 4 (Configuring Ubuntu and installing SSH.)
   1. First make sure the internet is connected via `WIFI` or `Ethernet`.
   2. Then just open Terminal and run this command to update and upgrade the OS.
        ````bash
        sudo apt update && sudo apt upgrade -y
        ````
   3. Installing `SSH`
        ````bash
        sudo apt install openssh-server -y
        ````
        - It takes a while to install.
   4. Verifying `SSH` is running.

        ````bash
        sudo systemctl status ssh
        ````
        - It shows Active (running).
        - Means it working fine.
   5. IF it can't works it is generally of Firewall issue.

        - Check UFW status:
        ````bash
        sudo ufw status
        ````
        If UFW is inactive, you'll see a message like "Status: inactive".

        - Enable UFW (if inactive):
        ````bash
        sudo ufw enable
        ````
        - Allow SSH through the firewall:
        ````bash
        sudo ufw allow ssh
        ````
        - Verify UFW rules:
        ````bash
        sudo ufw status
        ````
        You should now see SSH listed as allowed. 
----
> #### Step 5 (Testing SSH.)
1. On the Ubuntu(node) terminal run this command to Know it's `IP`.
````bash
ifconfig
````
2. Take a personal laptop or computer of Windows/Linux.
    - Open its Shell/Terminal and type this command.
    ````bash
    ssh <username-of-node@IP-of-node>
    ````
    - `username` we set on the time of Ubuntu installation.
3. Then is show some massage if we are connecting for first time.
    - Just type `yes` and press Enter.
    - Then it ask for `password`.
    - Enter the `password` we set on the time of installation.
4. After it gives a Shell of node.

#### ✅ Now we can Access the terminal Window of Node.
#### 👉 Repeat These steps for Each nodes and configure them.
#### 🧭 Next we can see the concept of SSH and ideology.
</br>

## 🥳 Done
## 😊 Hear We have Done the week one of building OWN K8s Physical-Server 

## By. [Yazish Khan](https://www.linkedin.com/in/yazish-khan-3634752b7?utm_source=share&utm_campaign=share_via&utm_content=profile&utm_medium=android_app)