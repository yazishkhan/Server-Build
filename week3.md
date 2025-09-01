## 🚀 Welcome to Week 3
<br>

## 💡 Today we are understanding the ideology of `ssh` and password less `ssh` access using `ssh` keys 

#### 1. What is SSH ?
SSH, or Secure Shell, constitutes a cryptographic network protocol designed to enable secure communication between two systems over networks that may not be secure. This protocol is widely employed for remote access to servers and the secure transmission of files between computers. In essence, SSH acts as a secure conduit, establishing a confidential channel for communication in scenarios where the network may pose security risks. This technology is instrumental for professionals seeking a reliable and secure method of managing servers and transferring sensitive data across computers in a controlled and protected manner. ssh runs at TCP/IP port 22. 
 
 - In simple meaning it is just a protocol/tool to connect the server by shell.
 - We can do any task on server by using shell access.
    --> Running scripts 

    --> Debugging problems

    --> Monitoring 

#### 2. Example.
##### This is an shell of my machine. 
![image](https://github.com/user-attachments/assets/695777b8-b8b5-4491-bd26-a47e566463d1)


##### This is an shell of the server when we ssh into it using `Password` authentication.
![image](https://github.com/user-attachments/assets/ef9c0185-9f35-4e05-a082-a2d94c911ef9)

##### This is how `ssh` works.

#### 3. Steps to use `ssh`

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
3. There it show some massage if we are connecting for first time.
    - Just type `yes` and press Enter.
    - Then it ask for `password`.
    - Enter the `password` we set on the time of installation.
4. After it gives a Shell of node.

#### 4. Setting up the password less access using `ssh` keys.
#### What are SSH Keys?
The SSH (Secure Shell) is an access credential that is used in the SSH Protocol. In other words, it is a cryptographic network protocol that is used for transferring encrypted data over the network. The port number of SSH is 22. It allow users to connect with server, without having to remember or enter password for each system. It always comes in key pairs:

Public key - Everyone can see it, no need to protect it. (for encryption function).
Private key - Stays in computer, must be protected. (for decryption function).
#### simple analogy!
 - We create the ssh keys on master node using `ssh-keygen` command.
 - After processing this command it will generate the ssh keys `~/.ssh` 
 - Then we need to share the `.pub` key to all worker nodes.
 - Then whenever we ssh from master node to workers it will automatically authenticate without asking password. 
 - When we implement `Iac` tools like ansible which workes on ssh push base mechanism it is important to set the password less communication.
 