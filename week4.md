## 🚀 Welcome to Phase 4
<br>

## 💡 Setting up an **Ansible** which automates the management of systems and controls their desired state.
----
## What is Ansible ❓
Ansible provides open-source automation that reduces complexity and runs everywhere. Using Ansible lets you automate virtually any task. Here are some common use cases for Ansible:

- Eliminate repetition and simplify workflows

- Manage and maintain system configuration

- Continuously deploy complex software

- Perform zero-downtime rolling updates

Ansible uses simple, human-readable scripts called playbooks to automate your tasks. You declare the desired state of a local or remote system in your playbook. Ansible ensures that the system remains in that state.

----
#### ✨ Features
**Agent-less architecture**
Low maintenance overhead by avoiding the installation of additional software across IT infrastructure.

**Simplicity**
    Automation playbooks use straightforward YAML syntax for code that reads like documentation. Ansible is also decentralized, using SSH with existing OS credentials to access to remote machines.

**Scalability and flexibility**
    Easily and quickly scale the systems you automate through a modular design that supports a large range of operating systems, cloud platforms, and network devices.

**Idempotence and predictability**
    When the system is in the state your playbook describes, Ansible does not change anything, even if the playbook runs multiple times.

----
#### 🏰  Architecture
![Image](https://github.com/user-attachments/assets/24c31cf9-be9d-475b-85bf-dad24a85dd3b)

#### Components of Architecture
**Control node**
A system on which Ansible is installed. You run Ansible commands such as ansible or ansible-inventory on a control node.

**Inventory**
A list of managed nodes that are logically organized. You create an inventory on the control node to describe host deployments to Ansible.

**Managed node**
A remote system, or host, that Ansible controls.


----
#### 🔌 Working (In my words)

- Before Using `Ansible` You need to setup password less ssh explained on week3. If you are using cloud platform.so your all node is in same `Security group` and create all the instances with same `.pem` file for easy setup.
- It is generally a tool used in `IaC` in `DevOps` it uses to configure the infrastructure.
- Why we need it e.g. we have a server/cluster server it may be `on-prem` or `cloud`. It has 4 nodes and we want to run our application in containers so we need to install `docker` in all of this four nodes.
- So we can just log in on each node my ssh and installing docker it is definitely possible. but if it is about large scale e.g we had 100 nodes.
- It is not possible to install `docker` on each node one by on.
- It consumes time, generate human errors, some installation errors etc.
- So we just use `Ansible` if we had 100 nodes we just need to setup `Ansible` in only one node out of hundred which is called to be `Master node`or `control node`.
- `Ansible` have `push base mechanism` we don't need to setup agent on other remaining nodes.
- So how it works? We had installed Ansible on master node in master node it has a file called `inventory` in it we just need to setup an IP address of all node present in server.
- `Inventory` has its own syntax in `ini`/`yaml` and modules. some of important I have explained on this documentation and other extra you will find on [docs.ansible.com](https://docs.ansible.com/ansible/2.9/user_guide/intro_inventory.html#inventory) 
- Now we have entered the IP adders my using inventory syntax. So now our Ansible knows those all are the servers nodes/hosts.
- `Playbook` this is an `yaml` file we need to declare all the task on it. e.g if we want to install docker we need to declare all those things on it.
- To install `Docker` first we need to update The OS and install it using `apt` Which is Linux package manager.  
- So those all the steps are required to install the docker we all need to define it in `Playbook`
- whenever we run a playbook it searches for nodes address in `inventory` to execute those tasks present in `Playbook`.
- This example is in very small scale but really the Ansible have lots of plugins to automate the task and comes with grate power. 
- This is how the logical working of Ansible goes to operate.

----

#### 📥 Installation of Ansible
This process is only for Debian based Linux distro. if you have others please visit [docs.ansible](https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html)
- We just need to install Ansible in master node only.

````bash
 sudo apt update
 sudo apt install software-properties-common
 sudo add-apt-repository --yes --update ppa:ansible/ansible
 sudo apt install ansible
````
- Just run these commands it will automatically install Ansible.

----
#### 📝 Implementation of Inventory

**The inventory is divided into some included things**
- **Groups** The all connected host/nodes are divided into groups eg. Development and production groups.
- **Hosts/List of host's** We list an host inside the group with it's `username`,`IP`
- **Variables** are most referenced as the host and group values of a system’s inventory with defined configurations.

##### Types of Ansible Inventories

###### 1. Static Inventory.

A static inventory is a file that contains a fixed list of hosts and groups. It is usually written in INI or YAML syntax and is ideal for small or relatively stable environments.

###### 2. Dynamic Inventory
Used in dynamic environments like cloud providers where hosts can change frequently. Ansible can fetch the list of hosts dynamically using scripts or plugins.
An inventory that is generated dynamically at runtime using scripts or APIs. Commonly used for cloud environments (e.g., AWS, Azure, & GCP) where the list of hosts can change frequently.
##### Implementing inventory for our Server.
- Just `ssh` to your 
- It is generally a static type of inventory we are using for our server setup.
- Prerequisite :- We already have to set up a Password less ssh.
- The inventory is present on `/etc/ansible/hosts`.
- Just type a command to open a inventory.
    ````bash
    vim /etc/ansible/hosts
    ````
- The basic structure of inventory is look like.
    ![image](https://github.com/user-attachments/assets/f6571411-a470-45f4-acb9-3ea5d63f6de4)
- We just need to edit this and write all info about host which is need to connect by `ssh`.
- Just edit it like this.

    ![image](https://github.com/user-attachments/assets/69621dd6-206a-4bcc-b45f-72248644f8ed)
    
    - There we have 3 groups this is only defined by me. You can define as per your req and some of the variables.
    - The groups are `master`,`workers`,and `cldserver` which is comment out.
    - The groups are dived as per mine requirement.
    - In group `master` there is `localhost` so Ansible understand to execute the task on master node itself which is written in playbook or in command.
    - The `ansible_connection` is [behavioral inventory parameters](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html) refers to the connection type eg.`ssh`,`local` for that particular host in front of it is written.
    - The next group is `workers` in it we have an `IP` address which is of node's `IP` and `ansible_user` is also a [behavioral inventory parameters](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html) to define an user of specific node.
    - In our server we have 4 node which have diff username as per seen in image. if we have same username on all node we just define an `ansible_user` in variable section at bottom as shown in image so the all groups can get the variable and give to all hosts.
    - But we have diff username on all hosts so we need to specify it separately.
    - There is nest group `cldserver` their I am using some of the EC2 instances means we can also use it on hybrid server setup.
    - We are only needed to set up our on-prem server.
    - Next is [variables](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html#adding-variables-to-inventory) section with the tag of `all` so those all variable is going to apply on all groups and its hosts.
    - If we replace `all` tag with group name eg. `workers` means the variables are only going to apply on that particular group.  
    - `ansible_python_interpreter`
        Specifies the target host Python path. This is useful for systems with more than one Python or for systems where Python is not located at /usr/bin/python, such as *BSD, or where /usr/bin/python is not a 2.X series Python.  We do not use the /usr/bin/env mechanism because that requires the remote user’s path to be set correctly and also assumes the python executable is named python, where the executable might be named something like python2.6.
    - `ansible_connection` we had understood it before. but in variable section this is going to apply on all the groups.
    - (optional) if we make an 4 instance on cloud from same `.pem` file we just need that pem file on `master node` and we just specify it in inventory so the Ansible goes to connect it from that `.pem` file.
        ![image](https://github.com/user-attachments/assets/f6c2c88d-a565-4de4-bfa6-2300c31c9541)
        - This is as shown in image just replace `/home/yazish/.ssh/id_rsa` with your pem key file path.
        - It is general for cloud if we have already set up an passionless ssh we don't need this.
    - After editing the file just press `clt+o` and then `clt+x` to write out and exit from `vim` editor.
##### Validating inventory build successfully.
- We just need to `ping` all the host present in inventory by this command.
    ````bash
    ansible all -m ping
    ````
    - If we run this command for a first time after setting ansible it stuck and we need to type `yes` then it will show the response of first node and the you need to retype `yes` us up to you will got an response of all node.
    ![image](https://github.com/user-attachments/assets/bd4dd1c3-ccde-481c-bfba-f09678aafd93)
    - The `all` refers to all groups present in inventory.
    - `-m` parameter is used to define `modules` 
    - `ping` is an module of Ansible use in playbook and command to check the all host are up or not.
    - By replacing `ping` we can use multiple Ansible [modules](https://docs.ansible.com/ansible/latest/module_plugin_guide/index.html)

##### Using multiple inventories.

````bash
ansible all -m ping
````
- This command automatically calls the default repository `/etc/ansible/hosts`
- If we want to use different inventory for diff server 
````bash
ansible all -i <path-of-inventory> -m ping
````
- The `i` parameter is used to specify the inventory path.
- This command can execute these task on host's present on that particular inventory.

----



#### 💉 Concept of Ad-hoc commands.
**Understanding it in a simple way by some small example which I use to do task with Ansible `ad-hoc` commands. This is generally an imperative way to execute the task with Ansible.**

Ad-hoc commands are great for tasks you repeat rarely. For example, if you want to power off all the machines in your lab for Christmas vacation, you could execute a quick one-liner in Ansible without writing a playbook. An ad hoc command looks like this.
- For e.g we need to power off our server which have an four node so without writing an playbook for this simple task we use an feature of `ad-hoc` command.
- just we want to run an following command to execute the task.
- The `-a` parameter is used to specify the ad-hoc command.
- And the `-b` **become root user** is used when the command need root access to execute. 
- `-K` tells Ansible to prompt you for the privilege-escalation (sudo) password (**--ask-become-pass**).
    ````bash
    ansible workers -a "poweroff" -b -K
    ````
- **Note** if you are running this command on master node make sure to specify the group `workers` so that command will execute on all worker node if we put `all` instead of `workers` the error we got our master node off first and the Ansible goes down so other node can't poweroff.
- So that's way I divided the master node and workers node into a different groups.
- In the command if we specify the `all` with the inventory it will execute that task on host which is only present on that particular inventory.
- We can also use multiple command under ad-hoc command section the command may be which we use in `Linux` daily.
e.g
    ````bash
    ansible all -a "free -h" 
    ````
- `free -h` command does not need any sudo permission so we don't need to pass parameter like `-b`or`-K`.
- This command is use te see memory usage in all the node after running this command Ansible give a response like.
![image](https://github.com/user-attachments/assets/7c63e18e-50ef-49f2-a31a-255c29d9125a)
- This is how we can use ad-hoc commands.
----

#### 📖 Playbook and its modules.
Playbooks are automation blueprints, in `YAML` format, that Ansible uses to deploy and configure nodes in an inventory. This guide introduces you to playbooks and then covers different use cases for tasks and plays

- Executing tasks with elevated privileges or as a different user.

- Using loops to repeat tasks for items in a list.

- Delegating playbooks to execute tasks on different machines.

- Running conditional tasks and evaluating conditions with playbook tests.

- Using blocks to group sets of tasks.
    
##### Understanding the concept of playbook in my words.
All [modules](https://docs.ansible.com/projects/ansible/latest/collections/ansible/builtin/file_module.html) on modules page press `clt+f` to pen search box for web and search modules as per your requirement.
- Create a playbook file with `.yaml` extension and write it with modules and plugins to execute the tasks on server eg. `example.yaml` to create run this command and write on it. 
    ````bash
    vim example.yaml
    ````
- The playbook we write with an some plugins and modules to execute our required task.
- E.g. we have an server of 4 nodes so in it, we need to install the docker using Ansible.
- First we need to update those all hosts and then goes to install docker. 
- In Linux we generally use an `apt` package manager to install the utilities.
- So in playbook we need to write the script in `YAML` so there Ansible provides an lot of modules to execute the task. 
- E.g. ansible have `apt` module to install utilities,`file` module to manage file permission and owner,`ufw` module to open ports and manage it.
- With the collection of this modules and its parameters we can write an playbook to execute the task. 
- Let's see the example of playbook.
    ![image](https://github.com/user-attachments/assets/bd834919-4576-4e4d-b737-5ac8455b02d3)
    - The first keyword of playbook `name` define the name of playbook.
    - `hosts` defines the all host present in inventory which we have specify on the time of playbook execution in command.
    - `hosts` have an parameter of `all` we can replace it with group name as in inventory.
    - `become ` module use to execute the task in administrative way `sudo` remember we use `sudo` when we update of install any utility.
    so we need to use `become`.
    - `task` defines the task to execute step by step.
    - Then the `name` of task to execute.
    - Before install docker we need to update first using `apt` [module](https://docs.ansible.com/ansible/2.9/modules/apt_module.html#apt-module)
    - the parameter of `apt` module is `update_cache` the the Ansible automatically tells host to update the system.
    - This is the power of Ansible module and its parameter.
    - The next parameter of `apt` is `name` which finds an which we had use to specify the name of package.
        ````yaml
        apt:
         name: docker.io
         state: latest
         update_cache: yes
        ````
    - There we have to understand one thing only the package we are able to install by using `apt` module is the package which is present on `apt` package repository.
    - Then how to check the package is present on `apt` repository. just run this command.
        ````bash
        apt search docker # you can replace <docker> with package name
        ```` 
        ![image](https://github.com/user-attachments/assets/ff2abe09-1417-4eec-9891-473a45e009f0)
    - After running the command it will give a list. So search on list as per in image this method is able to apply to check the any package is available or not in `apt` repo.
    - If the package is available you can easily use a `apt` module to install that package. If not the you have to first add repository then you go to install. This process is also be done by the Ansible modules. 
    - Then we have an state `parameter` of `apt` module to define the version of package we can replace `latest` tag with version so that particular package will be install.
    - Then the next task to start the service of docker remember we use `systemctl` to start, stop or to enable , disable the services by using Ansible playbook we use `service` module to does that. 
        ````yaml
        - name: start docker
          service:
           name: docker
           state: started
           enabled: yes
        ````
    - As per all module it has an parameter of `name` to specify the name of service. `state` to define **start** or **stop** condition and `enabled` to define service is enabled or disabled. What does enable or disable mean ?
    - Enable refers whenever the machine restarts then the service is going to automatically start and disable mean service is running now but whenever the machine restart it may not go to automatically start.
##### This is how the playbook modules and its parameter works. 
-----

#### 🚂 How to run the Playbook 
step 1:
- Open terminal.
- Open the directory where the playbook is located.

step 2:
- Run this command.
    ````bash
    ansible-playbook exapmle.yaml
    ````
- Hear we don't specify any inventory so it will atomically call the default inventory file `/etc/ansible/hosts`.

step 3: (optional)
- If we are using multiple inventory we need to specify by suing `-i` parameter.
    ````bash
    ansible-playbook -i <path-of-inventory> example.yaml
    ````
- If we need to run the playbook from different directory we just need to replace playbook name with the path of playbook.
    ````bash
    ansible-playbook -i <path-of-inventory> <path-of-repository>
    ````

----
##### This is all about Ansible and its power to execute the task on hundreds of nodes by using simple inventory setup, playbook and some commands.
## 🥳 Done
## 😊 Hear We have Done the phase 4 of building OWN K8s On-Premises-Server 

## By. [Yazish Khan](https://www.linkedin.com/in/yazish-khan-3634752b7?utm_source=share&utm_campaign=share_via&utm_content=profile&utm_medium=android_app)