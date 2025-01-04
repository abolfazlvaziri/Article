![](https://cdn-images-1.medium.com/max/800/1*e-yhkBEHOIIRtw6KvnCfAw.jpeg)

In our [previous article](https://github.com/abolfazlvaziri/Article/blob/main/Ansible%3A%20The%20Magic%20of%20Automation-Introduction%20%26%20Installation.md), we introduced Ansible and covered the installation steps. In this article, we will delve into the structure of Ansible, examining key components such as Playbooks and Roles. By understanding these concepts, you can use Ansible more effectively for automating system management and easily automate your repetitive processes.

Ansible is an open-source automation tool that simplifies IT processes such as configuration management, application deployment, and task automation. As the demand for efficient and scalable infrastructure management grows, automation becomes essential. Ansible helps organizations streamline operations, reduce human error, and improve consistency across environments.

#### Ansible Architecture
- **Controller Node:** The machine where Ansible is installed. It manages and orchestrates communication with the target nodes.
- **Managed Nodes:** These are the systems (servers, devices) that Ansible controls. They can be running various operating systems and do not require an agent to be installed.

> **Communication Protocols**
> Ansible uses **SSH** for secure communication with managed nodes, ensuring that commands are executed securely without the need for additional software.

#### File and Directory Structure
```css
project_name/  
├── inventory/  
│   └── inventory.yaml  
├── playbooks/  
│   └── playbook.yaml  
└── roles/  
    └── nginx_role/  
        ├── tasks/  
        │   └── main.yml  
        ├── handlers/  
        │   └── main.yml  
        ├── templates/  
        │   └── nginx.conf.j2  
        └── vars/  
            └── main.yml
```

#### **Inventory File**
The inventory file is a crucial component that defines the managed nodes. It can be a simple text file or a dynamic inventory script, allowing users to define groups of hosts.

**Example:** Simple `inventory.yaml` file
```yaml
all:  
  hosts:  
    webserver1:  
      ansible_host: 192.168.1.10  
      ansible_user: your_username  
      ansible_ssh_private_key_file: /path/to/your/private/key  
    webserver2:  
      ansible_host: 192.168.1.11  
      ansible_user: your_username  
      ansible_ssh_private_key_file: /path/to/your/private/key  
  vars:  
    ansible_python_interpreter: /usr/bin/python3
```

#### **Playbooks**
Playbooks are YAML files that define a series of tasks to be executed on the managed nodes. They consist of:
- **Plays:** Maps hosts to tasks.
- **Tasks:** Actions to be performed (e.g., installing packages).
- **Handlers:** Special tasks that run when notified by another task.

**Example:** Simple `playbook.yaml` file

```yaml
---  
- name: Install and configure Nginx  
  hosts: all  
  become: yes  
  tasks:  
    - name: Update apt cache  
      apt:  
        update_cache: yes  
  
    - name: Install Nginx  
      apt:  
        name: nginx  
        state: present  
  
    - name: Start Nginx service  
      service:  
        name: nginx  
        state: started  
        enabled: yes  
  
    - name: Copy default Nginx configuration  
      template:  
        src: /path/to/local/nginx.conf.j2  
        dest: /etc/nginx/nginx.conf  
      notify:  
        - Restart Nginx  
  
  handlers:  
    - name: Restart Nginx  
      service:  
        name: nginx  
        state: restarted
```

**Modules**  
Modules are the building blocks of Ansible, providing specific functionalities to perform tasks. Ansible comes with a wide range of built-in modules for various operations such as ‘apt’ module we used in simple example.

**Types of Modules**
- **Package Management:** Install, update, or remove software packages.
- **File Management:** Manage files and directories on managed hosts.
- **System Management:** Handle user accounts, services, and system configurations.

**Writing Custom Modules**  
Ansible allows users to create custom modules in Python or any other language that can output JSON. This flexibility enables tailored solutions for unique requirements.

#### **Roles**
Roles are a way to organize playbooks into reusable components. They encapsulate related tasks, variables, and files, promoting modularity and reusability.

If we want to do the previous process using a `Role`, we will have such a structure:
```css
nginx_role/  
├── tasks/  
│   └── main.yml  
├── handlers/  
│   └── main.yml  
├── templates/  
│   └── nginx.conf.j2  
└── vars/  
    └── main.yml
```

1. Create `tasks/main.yml` file(`yaml` or `yml` both is correct!)
```yaml
---  
- name: Update apt cache  
  apt:  
    update_cache: yes  
  
- name: Install Nginx  
  apt:  
    name: nginx  
    state: present  
  
- name: Start Nginx service  
  service:  
    name: nginx  
    state: started  
    enabled: yes  
  
- name: Copy default Nginx configuration  
  template:  
    src: nginx.conf.j2  
    dest: /etc/nginx/nginx.conf  
  notify:  
    - Restart Nginx
```

2. Create `handlers/main.yml` file
```yml
---  
- name: Restart Nginx  
  service:  
    name: nginx  
    state: restarted
```

3. Create `templates/nginx.conf.j2` file
```yml
server {  
    listen 80;  
    server_name localhost;  
  
    location / {  
        root /var/www/html;  
        index index.html index.htm;  
    }  
}
```

4. Create `vars/main.yml` file
```yml
---  
nginx_user: www-data  
nginx_group: www-data
```

**Using the Role in Playbook**  
To use the Role in your playbook, create `playbook.yaml` as follows:
```yml
---  
- name: Deploy Nginx using Role  
  hosts: all  
  become: yes  
  roles:  
    - nginx_role
```

#### Command to Use Ansible Playbook
To execute the Ansible playbook and apply the Nginx role with the specified inventory, use the following command:
```sh
ansible-playbook -i inventory/inventory.yaml playbooks/playbook.yml
```

#### Error Management
**Strategies for Error Handling**
Ansible provides several strategies for managing errors, including retries and ignoring errors on specific tasks.

**Using Handlers**
Handlers are triggered by tasks when a change occurs and can be used to respond to errors or perform cleanup actions.

#### Best Practices
**Configuration and Organization**
- **Use Descriptive Names:** Name your playbooks, tasks, and variables clearly to enhance readability.
- **Keep Playbooks Simple:** Break down complex playbooks into smaller, manageable tasks or roles.

> **Documentation**  
> Documenting your playbooks and roles is essential for maintainability. Use comments and ‘README’ files to explain the purpose and usage of your automation scripts.

Ansible is a powerful automation tool that boosts efficiency and consistency in IT operations. By mastering its architecture and best practices, users can automate complex tasks effectively. Stay tuned for our next article, where we’ll explore advanced tricks and techniques to elevate your Ansible skills to the next level!

---

Medium: [https://medium.com/@abolfazl.vaziri](https://medium.com/@abolfazl.vaziri)  
Instagram: [https://instagram.com/abolfazlvaziriofficial](https://instagram.com/abolfazlvaziriofficial)  
Telegram Channel: [https://t.me/AVN_COMMUNITY](https://t.me/AVN_COMMUNITY)  
YouTube: [https://www.youtube.com/@abolfazlvaziri](https://www.youtube.com/@abolfazlvaziri)
