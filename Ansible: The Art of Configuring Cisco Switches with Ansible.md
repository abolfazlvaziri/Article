
![getimg_ai_img-Ji2urXu642EijspErkTfg](https://github.com/user-attachments/assets/b850c9f7-629c-4a73-ab1b-151cdb08574d)

Ansible is a powerful automation tool that simplifies the management and configuration of network devices. Automating Cisco switch configurations with Ansible not only saves time but also ensures consistency and reduces human errors, making it an indispensable skill for network professionals.

**If you’re new to Ansible, I recommend reviewing these foundational articles:**

- [Ansible: The Magic of Automation — Introduction & Installation](https://github.com/abolfazlvaziri/Article/blob/main/Ansible%3A%20The%20Magic%20of%20Automation-Introduction%20%26%20Installation.md)
- [Ansible: Mastering Its Structure for Effective Automation](https://github.com/abolfazlvaziri/Article/blob/main/Ansible%20Mastering%20Its%20Structure%20for%20Effective%20Automation.md)
- [Ansible: Unlocking Advanced Automation Techniques](https://github.com/abolfazlvaziri/Article/blob/main/Ansible%20Unlocking%20Advanced%20Automation%20Techniques.md)

---

### Initial Setup
Before diving into automation, ensure the following prerequisites are met:  
1. **SSH Access**: Ensure SSH access is enabled on your Cisco switches.  
2. **User Privileges**: Create users with the appropriate privileges for configuration tasks.  
3. **SSH Key Setup**: Generate and deploy SSH keys for secure, password-less access:

```sh
$ ssh-keygen -t rsa -b 2048  
$ ssh-copy-id user@switch_ip
```

---

### Core Ansible Modules for Cisco Switches
Ansible provides specialized modules for managing Cisco devices. The two most commonly used modules are:
- `ios_config`: Pushes configuration changes to Cisco switches.
- `ios_command`: Executes commands on Cisco devices and retrieves their output.

**Disabling Unnecessary Services**
```yaml
- name: Disable Unnecessary Services  
  ios_config:  
    lines:  
      - no lldp run  
      - no ip finger  
      - no ip http server  
      - no ip http secure-server
```

**Gathering Running Configuration**
```yaml
- name: Gather Running Configuration  
  ios_command:  
    commands:  
      - show running-config
```

---

### Creating and Executing Playbooks

#### Basic Playbook: Configuring VLANs on a Cisco Switch
In basic example, we configure VLAN 10 on a Cisco switch. Note that the host, username, and password are specified within the playbook itself.
```yaml
- name: Basic VLAN Configuration on Cisco Switch  
  hosts: switches  
  tasks:  
    - name: Create VLAN 10  
      ios_config:  
        lines:  
          - vlan 10  
          - name Sales  
        provider:  
          host: "{{ inventory_hostname }}"  
          username: "{{ ansible_user }}"  
          password: "{{ ansible_ssh_pass }}"  
          authorize: yes  
          auth_pass: "{{ ansible_become_password }}"
```

#### Advanced Playbook: Configuring VLANs with Inventory Management
In advanced example, we centralize the management of connection details in the inventory file (`inventory/host.yaml`). This approach eliminates redundancy and simplifies playbook maintenance.

**Inventory File** (`inventory/host.yaml`):
```yaml
   [switches]  
   192.168.1.111  
   192.168.1.112  
  
   [switches:vars]  
   ansible_connection=ansible.netcommon.network_cli  
   ansible_user=[USERNAME]  
   ansible_password=[PASSWORD]  
   ansible_become=yes  
   ansible_become_method=enable  
   ansible_become_pass=[ENABLE_PASS]  
   ansible_network_os=cisco.ios.ios  
   ansible_ssh_common_args='-o ConnectTimeout=240'
```
- `192.168.1.111`: The IP address of the Cisco switch.
- `ansible_connection=ansible.netcommon.network_cli`: Specifies the connection method as network CLI.
- `ansible_user`: SSH username.
- `ansible_password`: SSH password.
- `ansible_become=yes`: Enables privilege escalation.
- `ansible_become_method=enable`: Uses the `enable` method for privilege escalation.
- `ansible_become_pass`: Password for privilege escalation.
- `ansible_network_os=cisco.ios.ios`: Specifies the network OS as Cisco IOS.
- `ansible_ssh_common_args=’-o ConnectTimeout=240'`: Sets the SSH connection timeout to 240 seconds.

**Playbook Content**:
```yaml
   - name: Advanced VLAN Configuration on Cisco Switch  
     hosts: switches  
     tasks:  
       - name: Configure VLAN 10  
         ios_config:  
           parents: "vlan 10"  
           lines:  
             - name Sales
```
- `parents` use for `Interfaces` too!

---

### Advanced Configuration Topics

#### Backup and Recovery
Regular backups of switch configurations are critical for disaster recovery. Use the following playbook to automate backups:
```yaml
- name: Backup Switch Configuration  
  hosts: switches  
  tasks:  
    - name: Capture running configuration  
      ios_command:  
        commands: show running-config  
      register: backup  
  
    - name: Save backup to file  
      copy:  
        content: "{{ backup.stdout }}"  
        dest: "/backup/switch_{{ inventory_hostname }}.cfg"
```
-  Store backups in a structured directory and schedule periodic backups using `cron` or `Ansible Tower`.

---

#### Configuring DHCP and DNS on Cisco Switches
Automate the configuration of DHCP and DNS services on your switches:
```yaml
- name: Configure DHCP Server on Cisco Switch  
  hosts: switches  
  tasks:  
    - name: Configure DHCP pool for VLAN 10  
      ios_config:  
        lines:  
          - ip dhcp pool VLAN10  
          - network 192.168.10.0 255.255.255.0  
          - default-router 192.168.10.1  
          - dns-server 192.168.10.1
```

> Other Configs has same structure!

---

### Using Tags in Playbooks
Tags allow you to selectively run specific tasks within a playbook. You can apply tags at the task or play level.

**Example Playbook with Tags**
```yaml
- name: Example Configure  
- hosts: switches  
  gather_facts: false  
  tasks:  
    - name: Configure EIGRP  
      ios_config:  
        src: "eigrp_template.j2"  
        match: none  
      tags:  
        - eigrp  
        - template  
        - tag1  
  
    - name: Configure Hostname  
      ios_config:  
        lines: hostname {{ inventory_hostname }}  
      tags:  
        - hostname  
        - tag1
```

**Running Playbooks with Tags**  
- List all tasks:  
     `ansible-playbook tags.yaml --list-tasks`
- List all tags:  
     `ansible-playbook tags.yaml --list-tags`
- Run specific tasks by tag:  
    `ansible-playbook tags.yaml --tags hostname`
    `ansible-playbook tags.yaml --tags eigrp`    
    `ansible-playbook tags.yaml --tags “eigrp,hostname”`
- Skip specific tasks by tag:  
    `ansible-playbook tags.yaml --skip-tags eigrp`
    `ansible-playbook tags.yaml --skip-tags “eigrp,hostname”`

---

#### Conditional Tasks
Use conditional logic to execute tasks based on specific conditions:

**File** `group_vars/all.yml`:
```yaml
ntp_enabled: true
```

**Playbook**:
```yaml
- name: Conditional NTP Configuration  
  hosts: switches  
  vars_files:  
    - group_vars/all.yml  
  tasks:  
    - name: Check if NTP should be configured  
      ios_config:  
        lines:  
          - ntp server 10.180.25.36  
      when: ntp_enabled | default(false)
```
- In this example if `ntp_enabled` is empty or any things instead of `true`, It is not possible and will be rejected(skip)!

---

#### Attention to Errors
Paying attention to errors is critical. Carefully reading and understanding them helps you quickly resolve issues.

Here’s an example of an SSH connection error:
```sh
fatal: [192.168.1.102]: FAILED! => {"changed": false, "msg": "ssh connection failed: ssh connect failed: No route to host"}
```
- Analyze and address such errors swiftly to maintain smooth network operations.
- The solution to this problem is to enable SSH, create a user with high privilege, and finally check the ACCESS-List related to the SSH device.

Mastering Ansible is a game changer for achieving seamless, high quality network integration. With Ansible, you can swiftly and accurately deploy all network configurations, including security setups, using intelligent playbooks. This empowers you to streamline your processes, reduce errors, and enhance the efficiency of your network operations.  
By embracing Ansible, you position yourself at the forefront of network automation, ensuring that your configurations are both robust and reliable.

> Start automating today and unlock the full potential of your network!

---

Medium: [https://medium.com/@abolfazl.vaziri](https://medium.com/@abolfazl.vaziri)  
Instagram: [https://instagram.com/abolfazlvaziriofficial](https://instagram.com/abolfazlvaziriofficial)  
Telegram Channel: [https://t.me/AVN_COMMUNITY](https://t.me/AVN_COMMUNITY)  
YouTube: [https://www.youtube.com/@abolfazlvaziri](https://www.youtube.com/@abolfazlvaziri)
