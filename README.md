# Ansible
Ansible Projects
These are my own notes to start with Ansible.

Configuration management is how we manage the configuration of the servers or the infrastructure.

Ansible is an open-source automation tool used for configuration management, application deployment, and orchestration. It helps automate IT tasks 
like installing software, managing servers, and deploying applications across multiple systems.

Orchestration in DevOps refers to automating and managing multiple processes across different environments. It ensures that various tasks (such as 
deployments, scaling, monitoring, and networking) work together efficiently.

Why is Orchestration Important?
    Reduces manual effort → Automates deployments, rollbacks, and updates.
    Ensures consistency → No configuration drift across environments.
    Speeds up delivery → Faster time-to-market by automating workflows.
    Improves scalability → Easily scale infrastructure and applications.
    Enhances reliability → Reduces human error in critical processes.

Example of Orchestration Tools
    Kubernetes → Orchestrates containerized applications.
    Terraform → Automates cloud infrastructure provisioning.
    AWS CodePipeline → Automates CI/CD pipelines.
    Ansible, Puppet, Chef → Automates server configuration and deployments.

Why Use Ansible?
    Agentless → No need to install anything on managed nodes (it uses SSH or WinRM).
    Simple & Easy to Learn → Uses YAML-based playbooks.
    Idempotent → Ensures the same result no matter how many times it's run.
    Scalable → Can manage thousands of machines at once.

What is Ansible Used For?

1️⃣ Configuration Management
    Automate setting up servers with the correct software, users, and settings.
    Example: Install and configure Nginx on multiple servers.

2️⃣ Application Deployment
    Deploy applications consistently across different environments.
    Example: Deploy a Docker container or a web app to AWS.

3️⃣ Orchestration
    Manage complex workflows (e.g., rolling updates, multi-tier applications).
    Example: Deploy an app to Kubernetes while updating a database.

4️⃣ Infrastructure as Code (IaC)
    Automate cloud resource provisioning (EC2, S3, RDS, etc.).
    Example: Use Ansible to set up AWS EC2 instances and configure networking.

5️⃣ Security & Compliance Automation
    Apply security patches, enforce firewall rules, and set up monitoring.
    Example: Ensure all Linux servers have the latest security updates.

How Does Ansible Work?
    Control Node → The machine where Ansible runs.
    Managed Nodes → The machines Ansible configures (Linux, Windows, etc.).
    Inventory File → A list of managed nodes (IP addresses or domain names).
    Playbooks → YAML files that define automation tasks.

System administrators used to do these with shell scripts but it was difficult to patch thousands of servers quickly. This is where Ansible comes 
into play.

Now with cloud systems we have tens of thousands of servers to manage and it is very difficult to manage them with shell scripts.

The hardest part was to create programs with various programming languages for various operating systems with various versions. To address this it 
was necessary to create a software for this.

So the concept of configuration management was created for this. Configuration management aims to solve the problem of managing the configuration 
of multiple servers. There are popular tools for this like Pupper, Chef, Ansible, and Salt. Ansible has most number of users or DevOps engineers.

Why is Ansible better then the others?

Pupper has a pull mechanism whereas Ansible has a push mechanism. For example a DevOps engineer pushes the configuration from his laptop to 10 EC2 
instances that he manages on AWS.

Puppet has a master/slave architecture whereas Ansible has an agentless model. With Puppet the developers has 1 master and many slaves. With 
Ansible we just place the names of the servers in n inventory file. The names can be IP addresses or DNS names. With Ansible out laptop should be 
able to access these instances without a password. So we need passwordless authentication. 

Ansible support dynamic inventory files. Which mean when new servers are created in out environment, Ansible can detect them and starts 
configuring them along with our other servers.

Ansible supports both Windows and Linux. But people have problems with Ansible on Windows.

Ansible is pretty simple. It uses yaml language. But Puppet has its own Puppet language.

There is room for improvement for debugging with Ansible. And it has performance issues sometimes.

Ansible is not just for configuration management but also for orchestration, application deployment, and security automation. Puppet is mainly 
focused on configuration management.

Ansible integrates easily with AWS, Azure, GCP, Docker, Kubernetes, and CI/CD tools.

Puppet has cloud modules but is less flexible for orchestration.

Ansible relies on SSH (which is already secure), while Puppet requires TCP ports to be open for communication.

We can write our own Ansible modules. Ansible is written in python. 

We can use Ansible to automate the configuration and management of load balancers like F5 BIG-IP, Nginx, HAProxy, and AWS ELB. This is useful in 
DevOps workflows where you need to:

✅ Deploy new servers behind a load balancer

✅ Automatically update configurations

✅ Scale infrastructure dynamically

Ansible manages load balancers using modules specific to each type. These modules allow us to configure virtual servers, pools, and policies 
dynamically. AWS provides Ansible modules for managing ELB resources.

F5 BIG-IP: Ansible modules handle enterprise-grade load balancing.

We connect to our EC2 instance on AWS with the command “ssh -i ansibleproject.pem ubuntu@18.199.220.57”.

Now we are connected to our Ubuntu EC2 instance and we can install Ansible. First we execute “sudo apt update” and then “sudo install ansible”. And we check it with “ansible –version”.

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

So let’s say we have a task of creating files in the target server. As this is a simple task we don’t need playbooks here. We can just run some simple commands. They are called “Ansible adhoc 
commands”.

How do we run this commands? We need an inventory file in our main machine that has the private IP addresses of our targets. We create it with “vim inventory” and write the private IP address of 
our target machine in it. We execute “ansible -i inventory IP_address_of_the_target_machine -m "shell" -a "touch devopsclass"”. Instead of the IP address we can just type all for all the machines 
in the inventory file. “-m” stands for module and “-a” stands for the shell commands.

We can see if the file is created on the target machine with “ls -ltr”. The command ls -ltr in Linux is used to list files and directories with detailed information, sorted by modification time in 
reverse order (oldest first).

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

For verbose mode we can use “ansible-playbook -vvv -i inventory first-playbook.yml”. Instead of 3 Vs we can use 1 as well. 3 Vs increases the verbosity (debug mode). This also gives the JSON file 
that tells what Ansible has done.

We can use Ansible to configure our Kubernetes Clusters as well.

Ansible Roles is an efficient way of writing Ansible playbooks that will only improve the efficiency of writing complex playbooks.

“ansible-galaxy role init kubernetes” command creates a role for Kubernetes. This command creates a folder named kubernetes with many files in it. “meta” folder has the metadata. Metadata is "data about data"—it provides information that describes other data. It helps in organizing, managing, and understanding data more efficiently. In the folder “default” we store some variables.
