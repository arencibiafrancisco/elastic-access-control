# Elastic Access Control Role

## Description

This Ansible role, `elastic_access`, is used to manage users in Elastic Cloud. It allows creating, updating, or deleting users based on the state defined in the users file.

## Prerequisites
- Ansible installed on your machine.
- Access to Elastic Cloud with an administrative user.
- SMTP server configuration if you want to send emails with user credentials.
Role Structure

```
roles/
└── elastic_access/
    ├── tasks/
    │   └── main.yml
    ├── vars/
    │   ├── elastic_cloud.yml   # Elastic Cloud configuration
    │   ├── smtp.yml            # SMTP configuration
    │   └── users.yml           # List of users with their states and roles
```

## Variables
Elastic Cloud Variables (`vars/elastic_cloud.yml`)
```
elastic_access_cloud_host: "Xlastic-cloud.com"
elastic_access_cloud_api_key: "/Y="
elastic_access_cloud_password: "your_secure_password"
elastic_access_password_length: 12
```

SMTP Variables (`vars/smtp.yml`)
```
elastic_access_smtp_server: smtp.office365.com
elastic_access_smtp_port: 587
elastic_access_smtp_user: notifications@yourdomain.com
elastic_access_smtp_password: "password"
elastic_access_from_email: notifications@yourdomain.com
```

User File Format (`vars/users.yml`)

The `users.yml` file contains the list of users to manage. Each user has the following properties:

```
- `username`: Username.
- `full_name`: Full name of the user.
- `email`: User's email address.
- `roles`: List of assigned roles.
- `state`: Defines whether the user should be **created/updated** (`present`) or **deleted** (`absent`).
```

Example:

```
elastic_access_users:
  - username: rhysmeister
    full_name: Rhys Campbell
    email: rhysmeister@example.com
    roles:
      - viewer
    state: present  # Creates or updates the user

  - username: janedoe
    full_name: Jane Doe
    email: janedoe@example.com
    roles:
      - user
    state: absent  # Deletes the user if it exists
```

## Role Usage

1. Ensure all variables are correctly set in their respective files under the `vars/` directory.
2. Create a playbook that calls the role:

```
- name: Manage Elastic Cloud users
  hosts: localhost
  gather_facts: false
  vars_files:
    - "{{ role_path }}/vars/elastic_cloud.yml"
    - "{{ role_path }}/vars/smtp.yml
  roles:
    - elastic_access
```

3. Run the playbook:

```
ansible-playbook manage_elastic_cloud.yml --ask-vault-password
```

Notes
- **`state: present`**: Creates or updates a user in Elastic Cloud.
- **`state: absent`**: Deletes a user if it exists.

Emails will be sent automatically with credentials if email sending is configured in the role tasks.
