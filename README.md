# 🚀 Enterprise Linux Automation with Ansible

> A hands-on DevOps project demonstrating multi-node Linux administration, AWS EC2 automation, configuration management, system monitoring, package management, file management, and infrastructure automation using Ansible.

---

## 🏗️ Project Architecture

```text
                    ┌─────────────────────────────┐
                    │      Ansible Controller     │
                    │                             │
                    │  Inventory                  │
                    │  Playbooks                  │
                    │  Variables                  │
                    │  Ansible Modules            │
                    └──────────────┬──────────────┘
                                   │
                              SSH / Ansible
                                   │
                    ┌──────────────┴──────────────┐
                    │                             │
             ┌──────▼───────┐             ┌──────▼───────┐
             │ Sample Node 1│             │ Sample Node 2│
             │    Ubuntu    │             │    Ubuntu    │
             │   AWS EC2    │             │   AWS EC2    │
             └──────────────┘             └──────────────┘
```

---

## 🎯 Project Overview

This project demonstrates practical use of **Ansible for automating Linux servers**.

The environment consists of:

- 1 Ansible Controller
- 2 Ubuntu managed nodes
- AWS EC2 infrastructure
- SSH-based Ansible communication

The automation covers server configuration, application setup, system information, package installation, file transfer, disk monitoring, and troubleshooting.

---

## 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| Ansible | Automation & Configuration Management |
| AWS EC2 | Cloud Infrastructure |
| Ubuntu Linux | Managed Operating System |
| SSH | Secure Server Communication |
| YAML | Playbook Configuration |
| Python | Ansible Runtime |
| Git | Version Control |
| GitHub | Source Code Management |
| MobaXterm | SSH Client & Terminal |

---

## 📂 Project Structure

```text
ansible-automation/
│
├── inventory/
│   └── hosts
│
├── playbooks/
│   ├── advanced-variables.yml
│   ├── application-config.yml
│   ├── system-information.yml
│   ├── file-management.yml
│   └── disk-space.yml
│
├── group_vars/
│   └── all.yml
│
├── screenshots/
│   ├── 01.jpg
│   ├── 02.jpg
│   ├── 03.jpg
│   ├── 04.jpg
│   ├── 05.jpg
│   ├── 06.jpg
│   ├── 07.jpg
│   ├── 08.jpg
│   ├── 09.jpg
│   ├── 10.jpg
│   ├── 11.jpg
│   ├── 12.jpg
│   ├── 13.jpg
│   ├── 14.jpg
│   └── 15.jpg
│
└── README.md
```

---

# 🔌 Ansible Inventory

Example inventory:

```ini
[appservers]
samplenode1
samplenode2

[appservers:vars]
ansible_user=devopsadmin
```

The inventory defines the servers that are managed by the Ansible Controller.

---

# 🔐 Ansible Connectivity

Ansible communicates with the managed nodes using SSH.

Connectivity was verified using:

```bash
ansible all -m ping
```

Example:

```text
samplenode1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}

samplenode2 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

---

# 🧩 Ansible Variables

Application configuration was managed using variables.

Example:

```yaml
app_name: myapp
app_version: "1.5"
app_user: devopsadmin
app_directory: /opt/myapp
```

Using variables makes the playbooks reusable and reduces hardcoded configuration.

---

# 👤 Application User Creation

The Ansible `user` module creates the application user.

```yaml
- name: Create application user
  ansible.builtin.user:
    name: "{{ app_user }}"
    state: present
    shell: /bin/bash
```

---

# 📁 Application Directory

The `file` module creates and manages the application directory.

```yaml
- name: Create application directory
  ansible.builtin.file:
    path: "{{ app_directory }}"
    state: directory
    owner: "{{ app_user }}"
    group: "{{ app_user }}"
    mode: "0755"
```

Example:

```text
/opt/myapp
```

---

# 📄 Dynamic Application Configuration

The project generates an application configuration file using Ansible variables.

```yaml
- name: Create configuration file
  ansible.builtin.copy:
    dest: "{{ app_directory }}/app.conf"
    content: |
      APPLICATION_NAME={{ app_name }}
      APPLICATION_VERSION={{ app_version }}
      APPLICATION_USER={{ app_user }}
      APPLICATION_DIRECTORY={{ app_directory }}
```

Generated configuration:

```text
APPLICATION_NAME=myapp
APPLICATION_VERSION=1.5
APPLICATION_USER=devopsadmin
APPLICATION_DIRECTORY=/opt/myapp
```

---

# 📦 Package Management

Required packages were installed using the Ansible package module and loops.

```yaml
- name: Install required packages
  ansible.builtin.package:
    name: "{{ item }}"
    state: present
  loop:
    - git
    - curl
    - wget
```

This demonstrates:

- Package management
- Loops
- Idempotency
- Automated configuration

---

# 🖥️ Ansible Facts

Ansible Facts were used to collect information about the managed nodes.

Examples:

```yaml
{{ ansible_distribution }}
{{ ansible_distribution_version }}
{{ ansible_architecture }}
{{ ansible_processor_vcpus }}
{{ ansible_memtotal_mb }}
{{ ansible_memfree_mb }}
```

Example output:

```text
OS Distribution: Ubuntu
OS Family: Debian
Architecture: x86_64
CPU Cores: 2
```

---

# 🔎 Gathering Specific Facts

Specific system facts can be retrieved using:

```bash
ansible samplenode2 -m setup -a "filter=ansible_distribution"
```

Example:

```text
samplenode2 | SUCCESS => {
    "ansible_facts": {
        "ansible_distribution": "Ubuntu",
        "discovered_interpreter_python": "/usr/bin/python3.12"
    },
    "changed": false
}
```

---

# 💻 CPU & Memory Information

The playbook displays CPU and memory information using Ansible Facts.

```yaml
- name: Display CPU information
  ansible.builtin.debug:
    msg:
      - "CPU Cores: {{ ansible_processor_vcpus }}"
      - "CPU Architecture: {{ ansible_processor[1] | default('Unknown') }}"
```

Memory:

```yaml
- name: Display memory information
  ansible.builtin.debug:
    msg:
      - "Total Memory: {{ ansible_memtotal_mb }} MB"
      - "Free Memory: {{ ansible_memfree_mb }} MB"
```

---

# 💾 Disk Space Monitoring

Disk usage was checked using:

```yaml
- name: Check disk space
  ansible.builtin.shell:
    cmd: df -h /
  register: disk_output
  changed_when: false
```

Example result:

```text
Filesystem      Size  Used  Avail  Use%  Mounted on
/dev/root       6.8G  2.4G  4.4G   36%   /
```

The command output is stored in:

```text
disk_output
```

and displayed using the `debug` module.

---

# 📋 Automated System Report

A system report was generated containing:

```text
System Information
==================

Hostname:
OS:
OS Version:
Architecture:
CPU Cores:
Total Memory:
Free Memory:
```

The report is created on the managed node:

```text
/tmp/system_report.txt
```

---

# 📤 File Copy Automation

The Ansible `copy` module was used to transfer files from the Controller to managed nodes.

Example:

```bash
ansible samplenode1 -m copy \
  -a "src=/etc/ansible/file1.txt dest=/home/ansibleadmin/file1.txt"
```

The resulting Ansible output provides details including:

```text
changed
checksum
destination
owner
group
mode
size
state
```

---

# 🔁 Ansible Loops

Loops were used to automate repetitive tasks.

Example:

```yaml
loop:
  - git
  - curl
  - wget
```

Instead of creating separate tasks for every package, a single task handles multiple packages.

---

# ⚙️ Conditional Execution

Conditional execution was demonstrated using:

```yaml
when: ansible_os_family == "Debian"
```

This allows tasks to execute only when the required condition is true.

---

# 🐛 Troubleshooting & Error Handling

During the project, several common Ansible and YAML issues were encountered and resolved.

### Issues Practiced

- YAML indentation errors
- Invalid YAML syntax
- Incorrect task indentation
- Unterminated quoted strings
- Incorrect playbook structure
- Host pattern mismatch
- Host matching issues
- Ansible command syntax errors
- Deprecated fact variable warnings

This provided practical experience in troubleshooting Ansible automation failures.

---

# ⚠️ Ansible Deprecation Warnings

The environment also demonstrated Ansible deprecation warnings related to automatically injected fact variables.

Modern Ansible syntax can use:

```yaml
ansible_facts["fact_name"]
```

instead of depending on automatically injected top-level facts where applicable.

---

# ▶️ Running the Automation

Check the inventory:

```bash
ansible-inventory --graph
```

Test connectivity:

```bash
ansible all -m ping
```

Gather all facts:

```bash
ansible all -m setup
```

Gather a specific fact:

```bash
ansible samplenode2 -m setup -a "filter=ansible_distribution"
```

Run a playbook:

```bash
ansible-playbook playbooks/advanced-variables.yml
```

Run with verbose output:

```bash
ansible-playbook playbooks/advanced-variables.yml -v
```

---

# 📊 Successful Play Recap

Example successful execution:

```text
PLAY RECAP

samplenode1    ok=8  changed=4  unreachable=0  failed=0
samplenode2    ok=8  changed=4  unreachable=0  failed=0
```

### Play Recap Explanation

```text
ok           = Successfully completed tasks
changed      = Tasks that made changes
unreachable  = Hosts Ansible could not connect to
failed       = Failed tasks
skipped      = Skipped tasks
rescued      = Rescued tasks
ignored      = Ignored failures
```

The successful execution demonstrates automation across both managed nodes.

---

# 📸 Project Screenshots

## 01 — Ansible Controller

![Ansible Controller](screenshots/01.jpg)

---

## 02 — Ansible Playbook Execution

![Playbook Execution](screenshots/02.jpg)

---

## 03 — Application Variables

![Application Variables](screenshots/03.jpg)

---

## 04 — Application Configuration

![Application Configuration](screenshots/04.jpg)

---

## 05 — Ansible Facts

![Ansible Facts](screenshots/05.jpg)

---

## 06 — Operating System Information

![Operating System Information](screenshots/06.jpg)

---

## 07 — CPU Information

![CPU Information](screenshots/07.jpg)

---

## 08 — Memory Information

![Memory Information](screenshots/08.jpg)

---

## 09 — Disk Space Monitoring

![Disk Space Monitoring](screenshots/09.jpg)

---

## 10 — System Report

![System Report](screenshots/10.jpg)

---

## 11 — File Copy Automation

![File Copy Automation](screenshots/11.jpg)

---

## 12 — Managed Node 1

![Managed Node 1](screenshots/12.jpg)

---

## 13 — Managed Node 2

![Managed Node 2](screenshots/13.jpg)

---

## 14 — Ansible Setup & Facts

![Ansible Setup](screenshots/14.jpg)

---

## 15 — Successful Automation

![Successful Automation](screenshots/15.jpg)

---

# 📚 Key Ansible Concepts

This project provided hands-on experience with:

- Ansible Controller
- Managed Nodes
- Inventory
- SSH
- YAML
- Variables
- Ansible Facts
- Modules
- `debug`
- `register`
- `loop`
- `when`
- `copy`
- `file`
- `user`
- `package`
- `shell`
- Idempotency
- Configuration Management
- Infrastructure Automation
- Linux Administration
- AWS EC2

---

# 💡 Key Learning

This project demonstrates how repetitive Linux administration tasks can be automated through reusable Ansible playbooks.

Instead of manually connecting to each server and performing the same configuration steps, the Ansible Controller can centrally manage multiple Linux nodes.

---

# 🚀 Project Outcome

Successfully implemented multi-node Linux automation using Ansible on AWS EC2.

The project strengthened practical skills in:

**Cloud + Linux + AWS + Ansible + Automation + Configuration Management + Infrastructure as Code**

---

# 👨‍💻 Author

## Mounesh Pattar

**Cloud & DevOps | Azure | AWS | Linux | Ansible | Automation**

---

# 🔖 Skills

`Ansible` `AWS` `Azure` `Linux` `Ubuntu` `EC2` `SSH` `YAML` `Python` `Git` `GitHub` `DevOps` `Cloud Computing` `Automation` `Configuration Management` `Infrastructure as Code`

---

# ⭐ Connect & Collaborate

If you found this project useful, feel free to ⭐ the repository and connect for Cloud & DevOps discussions.

#Ansible #DevOps #AWS #Azure #Linux #CloudComputing #Automation #ConfigurationManagement #InfrastructureAsCode #IaC #AWSCloud #CloudEngineer #DevOpsEngineer #LinuxAdministration #GitHub #AnsibleAutomation
