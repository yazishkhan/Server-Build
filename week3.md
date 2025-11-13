## 🚀 Welcome to Phase 3
<br>

## 💡 Today we are understanding the ideology of `ssh` and password less `ssh` access using `ssh` keys 

## 1. What is SSH ?
SSH, or Secure Shell, constitutes a cryptographic network protocol designed to enable secure communication between two systems over networks that may not be secure. This protocol is widely employed for remote access to servers and the secure transmission of files between computers. In essence, SSH acts as a secure conduit, establishing a confidential channel for communication in scenarios where the network may pose security risks. This technology is instrumental for professionals seeking a reliable and secure method of managing servers and transferring sensitive data across computers in a controlled and protected manner. ssh runs at TCP/IP port 22. 
 
 - In simple meaning it is just a protocol/tool to connect the server by shell.
 - We can do any task on server by using shell access.

    --> Running scripts 

    --> Debugging problems

    --> Monitoring 

## 2. Example.
##### This is an shell of my machine. 
![image](https://github.com/user-attachments/assets/695777b8-b8b5-4491-bd26-a47e566463d1)


##### This is an shell of the server when we ssh into it using `Password` authentication.
![image](https://github.com/user-attachments/assets/ef9c0185-9f35-4e05-a082-a2d94c911ef9)

##### This is how `ssh` works.

## 3. Steps to use `ssh`

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
3. There it shows some massage if we are connecting for first time.
    - Just type `yes` and press Enter.
    - Then it asks for `password`.
    - Enter the `password` we set on the time of installation.
4. After it gives a Shell of node.

## 4. Setting Up an static IP of all nodes inside the home network using Router.
1. Open routers `admin` page in any device it may mobile or any personal device.
    - Type the router's admin page IP on browser. Make sure the all nodes are connected to router.
    - I have D-Link router it has `192.168.0.1` IP/default gateway.
    - Then it shows the admin page.
    ![image](https://github.com/user-attachments/assets/4970eed4-5235-4944-ac70-bc45cd7804a3)
2. Look for `Network` section 
    - Inside the network section look for connected clients.
    ![image](https://github.com/user-attachments/assets/6f12ea1a-98a2-4ee9-9616-a8b46a628709)
    - click on it and it will give an list of all connected device and it's Ip and MAC address.
    ![image](https://github.com/user-attachments/assets/9cbb4ff6-1474-4020-911c-9c20b8399fa2)
3. Copy all nodes IP and MAC address from the table and save in any notepad.
    - Look for `Set static DHCP` and click on it.
    ![image](https://github.com/user-attachments/assets/9549f3a7-2750-4ee8-b02f-0a41551a4cda)
    - Enter the IP which we copied and Enter the MAC address by removing all the `:` between it's series.
    - Enter the IP and its own MAC address.
    - Then just save and apply it will set an static IP for node it can't change expect the router is reset.
    - Repeat these steps for all the remaining nodes and set it's IP static.
    - By setting up an static IP of all nodes under the home network is useful when we are implementing `Ansible` like tools which need ssh communication we need to define the all IP on master node inside the Ansible `inventory` file it is not possible to change the IP multiple times.

## 5. Setting up the password less access using `ssh` keys.
#### What are SSH Keys?
The SSH (Secure Shell) is an access credential that is used in the SSH Protocol. In other words, it is a cryptographic network protocol that is used for transferring encrypted data over the network. The port number of SSH is 22. It allows users to connect with server, without having to remember or enter password for each system. It always comes in key pairs:

Public key - Everyone can see it, no need to protect it. (for encryption function).
Private key - Stays in computer, must be protected. (for decryption function).
#### Simple analogy!
 - We create the ssh keys on `Master node` using `ssh-keygen` command.
 - After processing this command it will generate the ssh keys `.ssh` 
 - Then we need to share the `.pub` key to all `Worker nodes`.
 - Then whenever we ssh from master node to workers it will automatically authenticate without asking password. 
 - When we implement `Iac` tools like `Ansible` which Works on ssh push base mechanism it is important to set the password less communication.

 ## 6. Generating SSH keys.
 Setp 1:
 - Open terminal on `Master node` and type this command.
    ````bash
    ssh-keygen
    ````
    ![image](https://github.com/user-attachments/assets/4115c98c-d26b-494c-a477-bc2411bda59c)

 - Then it asks for file name do not enter any name just press `Enter` and `Enter`
 - It will generate the keys on directory `.ssh`
 - Then goes to Directory .ssh by
    ````bash
    cd ~/.ssh
    ````
 - Hear we see an ssh keys `id_ras` and `id_rsa.pub`  
 - We need to share the `.pub` keys to all worker nodes.
 - Remember do not share `id_rsa` to anyone.
----
Step 2:
- If the all worker node is under the same home network generally we need to use this command.
    ````bash
    ssh-copy-id ~/.ssh/id_rsa.pub yazish2@192.168.0.102
    ````
- The `ssh-copy-id` is a command which share the id on nodes.
- The `~/.ssh/id_rsa.pub` is an PATH of ssh key where it stored.
- The `yazish2@192.168.0.102` is an ssh address of worker node.
- This will automatically share and set the ssh key on worker node when we are running this command it asks password for last time and BOOM...
- Then just do the ssh to worker node from master. 
-----
Step 3: (optional)
- It is also called as Manuel method. If we need to share the ssh key out of the home network.
- Run this command and copy the contains.
    ````bash
    cat ~/.ssh/id_rsa.pub
    ````
    - This will look like `ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBxxxxxx...` just copy it.
    - Note : `id_rsa.pub` name may be differed please validate first otherwise command is not going o work 
- Just copy it.
- Login to the Server/Worker node.
- Navigate to `.ssh` folder.
    ````bash
    cd ~/.ssh
    ````
- Run this command and `past` the copied contain on it.
    ````bash
    nano authorized_keys
    ````
- Save and Exit by pressing `CLT + O` & `CLT + X`
- Give permission to `~/.ssh/authorized_keys` file.
    ````bash
    chmod 600 ~/.ssh/authorized_keys
    ````
- Give permission to `~/.ssh` folder.
    ````bash
    chmod 700 ~/.ssh
    ````

## 7. Verifying password-less ssh access.
##### Just do an ssh using this command it will not ask for password.

````bash
ssh <hots-name@IP>
`````
![image](https://github.com/user-attachments/assets/caef8fdb-c8d4-4c37-a475-debefeca45b0)

## This how ssh and it's password less mechanism works.

## 🥳 Done
## 😊 Hear We have Done the Phase 3 of building OWN K8s On-Premises-Server 

## By. [Yazish Khan](https://www.linkedin.com/in/yazish-khan-3634752b7?utm_source=share&utm_campaign=share_via&utm_content=profile&utm_medium=android_app)