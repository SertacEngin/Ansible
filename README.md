# Ansible
Ansible Projects
These are my own notes to start with Ansible.
We connect to our EC2 instance on AWS with the command “ssh -i ansibleproject.pem ubuntu@18.199.220.57”. 
Now ae are connected to our Ubuntu EC2 instance and we can install Ansible. First we execute “sudo apt update” and then “sudo install ansible”. And we check it with “ansible –version”.
We need 2 servers for this. First is the Ansible server and the other is the target Ubuntu server.
We will configure the target server using the Ansible server.
We need to authenticate into the target ubuntu. And then we can do anything in the target server.
Let’s set passwordless authentication. For that we use the private IP address.
We create a public/private key with “ssh-keygen”. So our id_ed25519.pub key has been created.
Then we connect to our target machine from another terminal.
And we execute “ssh-keygen” on the target machine as well.
And we open the authorized keys on this target machine. And we copy our id_ed25519 key from the Ansible machine and copy it into the authorized keys in the target machine.
Now we can connect to the target machine from our main machine with “ssh private_IP_address_of_the_target_machine”. 
This was the prerequisite for the Ansible. The passwordless authentication. And we ensured it now.
We can this for a second and third target machine as well.
We call Ansible files “playbooks”.
So let’s say we have a task of creating files in the target server. As this is a simple task we don’t need playbooks here. We can just run some simple commands. They are called “Ansible adhoc commands”.
How do we run this commands? We need an inventory file in our main machine that has the private IP addresses of our targets. We create it with “vim inventory” and write the private IP address of our target machine in it. We execute “ansible -i inventory IP_address_of_the_target_machine -m "shell" -a "touch devopsclass"”. Instead of the IP address we can just type all for all the machines in the inventory file. “-m” stands for module and “-a” stands for the shell commands.
We can see if the file is created on the target machine with “ls -ltr”. The command ls -ltr in Linux is used to list files and directories with detailed information, sorted by modification time in reverse order (oldest first).
We only use playbooks when we have a set of these commands. This is the difference between ansible adhoc commands and ansible playbooks.
We can refer to Ansible documentation for modules.
For example, “ansible -i inventory all -m “shell” -a “nproc”” will show us how many CPUs the target machine has.
“ansible -i inventory all -m “shell” -a “df”” for the disk usage.
For example we can copy the files from one server to the other. For that we can just search for a copy module in the ansible documentation.
Now let’s see what we can do when we have multiple servers.
We can group the private IP addresses of our target machines in the inventory file.
Now instead of using “all” we can specify a group of IP addresses:
“ansible -i inventory webservers -m "shell" -a "df"”. This will be executed on all the webservers.
This is how we group the servers in Ansible. We do it in the inventory files.
[webservers]
IP_address_1
IP_address_2
IP_address_3
Now let’s look at an example of a playbook. We want to install and restart Nginx. Nginx is open-source web server software used for reverse proxy, load balancing, and caching.
We create a playbook with “vim first-playbook.yml”. It is good practice to use linux commands instead if shell commands.
We use “ansible-playbook” to run Ansible playbook commands. “ansible-playbook -i inventory first-playbook.yml”.
[Gathering Facts] is the first tast being executed.
Then we can check on the target ubuntu if nginx is installed with “sudo systemctl status nginx”.
For verbose mode we can use “ansible-playbook -vvv -i inventory first-playbook.yml”. Instead of 3 Vs we can use 1 as well. 3 Vs increases the verbosity (debug mode). This also gives the JSON file that tells what Ansible has done.

We can use Ansible to configure our Kubernetes Clusters as well.
Ansible Roles is an efficient way of writing Ansible playbooks that will only improve the efficiency of writing complex playbooks.
“ansible-galaxy role init kubernetes” command creates a role for Kubernetes. This command creates a folder named kubernetes with many files in it. “meta” folder has the metadata. Metadata is "data about data"—it provides information that describes other data. It helps in organizing, managing, and understanding data more efficiently. In the folder “default” we store some variables.
