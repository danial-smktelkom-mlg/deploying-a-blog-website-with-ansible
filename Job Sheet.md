### **Job Sheet: Deploying a Blog Website with Ansible**

---

#### **Objective:**
This job sheet will guide you through the process of:
1. Setting up an inventory file and playbook.
2. Installing necessary packages (Apache, MariaDB, and PHP).
3. Creating a MySQL database and user.
4. Importing the SQL file into the database.
5. Deploying the website from the GitHub repository.

---

### **Step 1: Preparing Your Environment**

#### **Exercise 1.1: Creating the Inventory File**

Create an inventory file named `inventory.ini` with the following content:
```ini
[blog]
your_host_ip ansible_user=your_host_user ansible_ssh_private_key_file=/path/to/private/key.pem
```
- **Objective**: Define the server’s IP, host user, and SSH key.
- **Additional Exercise**: Modify the file to connect to a different server, if available.

#### **Exercise 1.2: Testing Ansible Connectivity**

Verify the Ansible connection using the command:
```bash
ansible -i inventory.ini blog -m ping
```
- **Objective**: Ensure Ansible can connect to the server.
- **Additional Exercise**: Use a different inventory or modify the connection parameters.

---

### **Step 2: Creating the Playbook**

#### **Exercise 2.1: Basic Playbook Setup**

Create a playbook named `playbook.yml`:
```yaml
- hosts: blog
  become: yes

  tasks:
    - name: Update package repository
      apt:
        update_cache: yes
```
- **Objective**: Start by updating the package repository.
- **Additional Exercise**: Add a task to install a simple package like `curl`.

#### **Exercise 2.2: Installing Required Packages**

Extend the playbook to install Apache, MariaDB, and PHP:
```yaml
    - name: Install Apache, MariaDB, and PHP
      apt:
        name:
          - apache2
          - mariadb-server
          - php
          - php-mysqli
          - php-mbstring
        state: present
```
- **Objective**: Automate the installation of the necessary packages.
- **Additional Exercise**: Verify the installations by checking the versions of each package.

---

### **Step 3: Managing MySQL**

#### **Exercise 3.1: Create MySQL Database and User**

Add tasks to create the MySQL database and user:
```yaml
    - name: Create MySQL database
      mysql_db:
        name: blog2
        state: present

    - name: Create MySQL user
      mysql_user:
        name: blog2
        password: blog2
        host: localhost
        priv: "blog2.*:ALL"
        state: present
```
- **Objective**: Automate the creation of the database and user for the blog.
- **Additional Exercise**: Verify by logging into MySQL and checking the created database and user.

#### **Exercise 3.2: Importing SQL File into Database**

Add a task to import the `db.sql` file into the `blog2` database:
```yaml
    - name: Import SQL file into database
      mysql_db:
        name: blog2
        state: import
        target: /path/to/db.sql
        login_user: blog2
        login_password: blog2
```
- **Objective**: Automate the import of the SQL file into the database.
- **Additional Exercise**: Verify by checking the contents of the `blog2` database.

---

### **Step 4: Deploying the Website**

#### **Exercise 4.1: Cloning the GitHub Repository**

Add a task to clone the blog repository from GitHub:
```yaml
    - name: Clone blog repository
      git:
        repo: https://github.com/danial-smktelkom-mlg/blog2.git
        dest: /var/www/html/blog2
```
- **Objective**: Automate the cloning of the blog repository to the server.
- **Additional Exercise**: Verify by checking if the files exist in `/var/www/html/blog2`.

#### **Exercise 4.2: Configure Apache for the Blog**

Update Apache configuration for the blog:
```yaml
    - name: Configure Apache for blog
      template:
        src: blog.conf.j2
        dest: /etc/apache2/sites-available/blog.conf

    - name: Enable the blog site
      command: a2ensite blog.conf

    - name: Restart Apache to apply changes
      service:
        name: apache2
        state: restarted
```
- **Objective**: Configure Apache to serve the blog website.
- **Additional Exercise**: Open the server IP in a browser to verify that the blog is accessible.

---

### **Final Practice: Full Playbook Execution**

To execute the full automation, combine all tasks and run:
```bash
ansible-playbook -i inventory.ini playbook.yml
```
- **Objective**: Automate the complete blog installation and configuration process.
- **Observation**: After running the playbook, the blog should be fully deployed and accessible.

---

### **Summary**

This job sheet provides a comprehensive guide for deploying a blog website using Ansible. Each section is designed to build your understanding through practical exercises, ensuring you gain hands-on experience. Review the objectives and complete the additional exercises to enhance your learning further.

---

### **Additional Recommendations:**

- Ensure that you have all necessary permissions to execute the tasks outlined in this job sheet.
- Familiarize yourself with Ansible documentation for any specific commands or modules you may not understand.
- Backup any existing configurations before making changes to your server.
