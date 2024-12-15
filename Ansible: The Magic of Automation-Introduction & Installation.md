![2](https://github.com/user-attachments/assets/ed85d3d2-d606-494f-b09f-8deb55763482)

Imagine a world where managing complex Network&OS configurations is as simple as writing a few lines of code. Ansible is a game-changer in the realm of IT automation. Whether you are a seasoned network engineer or a newcomer looking to enhance your skills, Ansible offers a streamlined way to manage and config Linux, Windows, Cisco equipment and etc. automate mundane tasks freeing you to focus on more strategic initiatives.

In this article, we will demystify the process of installing Ansible on both Windows and Linux systems. By the end, you’ll have the foundational knowledge to leverage this powerful tool in your Network&OS management toolkit. Say goodbye to tedious manual configurations and hello to a more efficient and secure network environment.

# What is Ansible?

Ansible is an open-source tool for automating IT processes. With Ansible, you can easily automate complex OS configuration and network management tasks. This tool allows you to manage operations effortlessly using playbooks and modules.

# Benefits of Ansible

- **Simple and User-Friendly**: Uses YAML for defining playbooks.
- **Agentless**: Ansible operates without installing additional software on target devices.
- **Scalable**: Capable of managing thousands of devices simultaneously.

# Installing Ansible on Windows

To install Ansible on Windows, you need to use Windows Subsystem for Linux (WSL). Follow these steps:

1. **Install WSL**: Open PowerShell as Administrator and run the following command:

```sh
wsl --install
```

2. **Install Ansible**: Open WSL and install a Linux distribution (like Ubuntu). In the WSL terminal, enter the following commands:

```sh
sudo apt update  
sudo apt install ansible
```

> You can also install Ubuntu from Microsoft-Store too!

# Installing Ansible on Linux

To install Ansible on Linux, follow these steps:

**Installation on Ubuntu**

```sh
# Update Repositories:  
sudo apt update  

# Install Ansible:  
sudo apt install ansible
```

**Installation on CentOS**

```sh
# Install EPEL Repository:  
sudo yum install epel-release  
   
# Install Ansible:  
sudo yum install ansible
```

In this article, we covered how to install Ansible on both Windows and Linux operating systems. In the next article, we will explore process automation and Ansible modules. Stay tuned as we delve deeper into the power of Ansible in managing Network&OS.

-----

GitHub: [https://github.com/abolfazlvaziri](https://github.com/abolfazlvaziri)  
Instagram: [https://instagram.com/abolfazlvaziriofficial](https://instagram.com/abolfazlvaziriofficial)  
Telegram Channel: [https://t.me/AVN_COMMUNITY](https://t.me/AVN_COMMUNITY)  
YouTube: [https://www.youtube.com/@abolfazlvaziri](https://www.youtube.com/@abolfazlvaziri)
