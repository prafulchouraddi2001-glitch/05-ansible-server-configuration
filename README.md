# Ansible Server Configuration

Automated web server configuration using **Ansible**, **AWS EC2**, **Nginx**, and reusable Ansible roles.

## Overview

This project demonstrates how Terraform and Ansible can be used together to provision and configure an AWS EC2 web server.

**Terraform** is responsible for provisioning the AWS infrastructure, while **Ansible** handles the configuration and management of the EC2 server.

The Ansible implementation was initially created as a traditional playbook and later refactored into a reusable role-based structure.

### Deployment Flow

```text
Terraform
   |
   | Provisions AWS EC2
   ▼
AWS EC2
   │
   │ SSH
   ▼
Ansible Controller
   │
   │ Ansible Playbook
   ▼
webserver Role
   ├── Install packages
   ├── Configure Nginx
   ├── Deploy Jinja2 templates
   ├── Create project directory
   └── Create application information file
   │
   ▼
Running Nginx Web Server
```

## Technologies Used

- Ansible
- AWS EC2
- Nginx
- Linux
- Bash
- YAML
- Jinja2
- Git
- GitHub
- Terraform

## Project Structure

```text
05-ansible-server-configuration/
├── ansible.cfg
├── inventory/
│   ├── hosts.ini
│   └── group_vars/
│       └── web.yml
├── playbooks/
│   └── site.yml
└── roles/
    └── webserver/
        ├── defaults/
        │   └── main.yml
        ├── handlers/
        │   └── main.yml
        ├── tasks/
        │   └── main.yml
        └── templates/
            ├── index.html.j2
            └── nginx.conf.j2
```

## What Ansible Configures

The `webserver` role:

1. Updates installed packages.
2. Installs Nginx and Git.
3. Enables and starts Nginx.
4. Configures Nginx using a Jinja2 template.
5. Deploys a custom HTML page.
6. Creates `/opt/devops-project`.
7. Creates `/opt/devops-project/info.txt`.
8. Uses an Ansible handler to reload Nginx when its configuration changes.

## Ansible Role

The main playbook is intentionally small:

```yaml
---
- name: Configure web server
  hosts: web
  become: true

  roles:
    - webserver
```

The configuration logic is separated into the reusable `webserver` role.

### Defaults

Role variables are defined in:

```text
roles/webserver/defaults/main.yml
```

Example variables include:

```yaml
nginx_package: nginx
git_package: git
web_service: nginx
project_directory: /opt/devops-project
deployment_environment: learning
```

### Templates

The role uses Jinja2 templates for:

- Nginx configuration
- The web page

The templates dynamically display the deployment environment and Ansible inventory hostname.

## Running the Project

From the project root:

```bash
ansible-playbook --syntax-check playbooks/site.yml
```

Then apply the configuration:

```bash
ansible-playbook playbooks/site.yml
```

## Verification

Check Nginx:

```bash
ansible web -m shell -a 'systemctl is-enabled nginx && systemctl is-active nginx'
```

Expected:

```text
enabled
active
```

Check the deployed application information:

```bash
ansible web -m shell -a 'cat /opt/devops-project/info.txt'
```

Test the web server:

```bash
curl http://<EC2_PUBLIC_IP>/
```

The deployed page displays information such as:

```text
Hello from Terraform + Ansible
Environment: learning
Host: terraform-web
```

## Key DevOps Concepts Demonstrated

- Infrastructure provisioning with Terraform
- Configuration management with Ansible
- Ansible inventories
- Ansible variables
- Jinja2 templating
- Ansible handlers
- Reusable Ansible roles
- Idempotent configuration
- Remote Linux administration
- AWS EC2 deployment
- Git version control

## Project Evolution

The project was first implemented as a traditional Ansible playbook.

It was then refactored into a reusable role:

```text
Playbook
   ↓
webserver Role
   ├── defaults
   ├── tasks
   ├── handlers
   └── templates
```

This separates the playbook from the configuration logic and makes the web server configuration easier to maintain and reuse across multiple servers.

## Verification Status

- Ansible syntax check: Passed
- Ansible playbook execution: Passed
- Nginx: Enabled and active
- Web page: Accessible over HTTP
- Application information file: Created
- Ansible role structure: Implemented
- Git working tree: Clean```markdown
# Ansible Server Configuration

Automated web server configuration using Ansible, AWS EC2, Nginx, and reusable Ansible roles.

## Overview

This project demonstrates how Ansible can automate the configuration of an AWS EC2 web server.

The project evolved from a basic Ansible playbook into a reusable role-based structure.

### Deployment Flow

```text
Terraform
   |
   | Provisions AWS EC2
   ▼
AWS EC2
   │
   │ SSH
   ▼
Ansible Controller
   │
   │ Ansible Playbook
   ▼
webserver Role
   ├── Install packages
   ├── Configure Nginx
   ├── Deploy Jinja2 templates
   ├── Create project directory
   └── Create application information file
   │
   ▼
Running Nginx Web Server
```

## Technologies Used

- Ansible
- AWS EC2
- Nginx
- Linux
- Bash
- YAML
- Jinja2
- Git
- GitHub
-Terraform

## Project Structure

```text
05-ansible-server-configuration/
├── ansible.cfg
├── inventory/
│   ├── hosts.ini
│   └── group_vars/
│       └── web.yml
├── playbooks/
│   └── site.yml
└── roles/
    └── webserver/
        ├── defaults/
        │   └── main.yml
        ├── handlers/
        │   └── main.yml
        ├── tasks/
        │   └── main.yml
        └── templates/
            ├── index.html.j2
            └── nginx.conf.j2
```

## What Ansible Configures

The `webserver` role:

1. Updates installed packages.
2. Installs Nginx and Git.
3. Enables and starts Nginx.
4. Configures Nginx using a Jinja2 template.
5. Deploys a custom HTML page.
6. Creates `/opt/devops-project`.
7. Creates `/opt/devops-project/info.txt`.
8. Uses an Ansible handler to reload Nginx when its configuration changes.

## Ansible Role

The main playbook is intentionally small:

```yaml
---
- name: Configure web server
  hosts: web
  become: true

  roles:
    - webserver
```

The configuration logic is separated into the reusable `webserver` role.

### Defaults

Role variables are defined in:

```text
roles/webserver/defaults/main.yml
```

Example variables include:

```yaml
nginx_package: nginx
git_package: git
web_service: nginx
project_directory: /opt/devops-project
deployment_environment: learning
```

### Templates

The role uses Jinja2 templates for:

- Nginx configuration
- The web page

The templates dynamically display the deployment environment and Ansible inventory hostname.

## Running the Project

From the project root:

```bash
ansible-playbook --syntax-check playbooks/site.yml
```

Then apply the configuration:

```bash
ansible-playbook playbooks/site.yml
```

## Verification

Check Nginx:

```bash
ansible web -m shell -a 'systemctl is-enabled nginx && systemctl is-active nginx'
```

Expected:

```text
enabled
active
```

Check the deployed application information:

```bash
ansible web -m shell -a 'cat /opt/devops-project/info.txt'
```

Test the web server:

```bash
curl http://<EC2_PUBLIC_IP>/
```

The deployed page displays information such as:

```text
Hello from Terraform + Ansible
Environment: learning
Host: terraform-web
```

## Key DevOps Concepts Demonstrated

- Infrastructure configuration automation
- Configuration management
- Ansible inventories
- Ansible variables
- Jinja2 templating
- Ansible handlers
- Reusable Ansible roles
- Idempotent configuration
- Remote Linux administration
- AWS EC2 deployment
- Git version control

## Project Evolution

The project was first implemented as a traditional Ansible playbook.

It was then refactored into a reusable role:

```text
Playbook
   ↓
webserver Role
   ├── defaults
   ├── tasks
   ├── handlers
   └── templates
```

This makes the configuration easier to maintain and reuse across multiple web servers.

## Verification Status

- Ansible syntax check: Passed
- Ansible playbook execution: Passed
- Nginx: Enabled and active
- Web page: Accessible over HTTP
- Application information file: Created
- Ansible role structure: Implemented
- Git working tree: Clean
```

