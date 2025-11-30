Ansible


This document contains:  1) Ansible project ideas for hands-on experience  2) Ansible FAQs 3) Ansible notes (Notes are also there in diary and PL notes)
 Project Ideas for Ansible: 

To copy SSH public key from control node to worker node we can use command: ssh-copy-id user@hostname . Example: ssh-copy-id root@172.30.2.2
 1) First thing I did with Ansible to practice was to set up my entire linux environment. I mean everything, bash setup, apps installed, themes, window manager, everything. Then I refactored it to do it to other pcs, so with that I can now get a new computer and set it up in 5 minutes by installing git, ansible and running a playbook. Its really fun and depending on your environment you might have to learn many things about ansible. Of course you wont be learning about complex inventories and different infrastructures there, but you will train your role writing skills while having some fun.  2) Here are ten projects you can do with Ansible:
Provision and configure servers: Ansible can automatically provision and configure servers, including setting up the operating system, installing software, and configuring network settings.
Deploy applications: Ansible can be used to deploy applications to servers, including web applications, databases, and middleware.
Manage infrastructure: Ansible can be used to manage infrastructure tasks, such as provisioning and configuring storage, networking, and cloud resources.
Automate security tasks: Ansible can automate security tasks, such as vulnerability scanning, patching, and configuration management.
Orchestrate complex IT processes: Ansible can be used to orchestrate complex IT processes, such as deploying applications to a production environment or rolling out new software updates.
Manage cloud infrastructure: Ansible can be used to manage cloud infrastructure, such as Amazon Web Services (AWS), Microsoft Azure, and Google Cloud Platform (GCP).
Automate DevOps workflows: Ansible can be used to automate DevOps workflows, such as continuous integration (CI) and continuous delivery (CD).
Manage network devices: Ansible can be used to manage network devices, such as routers, switches, and firewalls.
Automate configuration management: Ansible can be used to automate configuration management tasks, such as ensuring that all servers are configured in a consistent and compliant manner.
Automate testing and validation: Ansible can be used to automate testing and validation tasks, such as running automated tests and verifying that deployments have been successful.

3. Just search for any software you can selfhost. E.g. WordPress.
Look up the installation instructions and try to automate it with Ansible.  Ansible FAQs:
 
What is configuration management.
Previously it was easy to set up servers when the number of servers were limited, but with cloud as number of servers are increasing  updates, patching of systems, downloading softwares is automated using configuration management

What is cicd. You are doing config management manually or from cicd
1. What is CI-CD in DevOps? 
CI-CD stands for Continuous Integration and Continuous Delivery/Deployment, which automates code integration, testing, and delivery to production. 
2. What is Continuous Integration, CI? 
CI is the practice of frequently merging code changes into a shared repository with automated builds and tests to detect issues early. 
3. What is Continuous Delivery, CD? 
CD ensures that code changes are automatically prepared for deployment to production, often requiring manual approval before release. 
4. What is Continuous Deployment? 
Continuous Deployment automatically releases every code change that passes automated tests directly to production without manual intervention. 
5. What are CI-CD Pipelines? 
Pipelines are automated workflows that define the sequence of steps for building, testing, and deploying applications. 

Why ansible?
It is agent less, uses push model, written in python so easy to write modules if required, its tasks are written in YAML which is easy language

What is push and pull method.
In pull model the target servers keep sending a request after a fixed interval to check if there are any changes. In case of push model the changes are pushed from host to target servers.

What are ansible modules have you used it.

* Task: Defines the action to be applied to a managed host, but does not specify the host it will be applied to. You can invoke a module within a task to be executed on a host, but that host will be defined outside of the task, at the play level.
* Play: The core unit of Ansible execution, a play includes a list of tasks and maps that list to specific managed hosts. A play also contains variables and, in more complex use cases, may contain roles.
* Ansible Playbook: Includes 1 or more plays. Playbooks are written in YAML, are human-readable, and easy to share.
* Ansible Role: Packages Ansible content—including tasks, handlers, variables, plugins, templates, and files—for use inside of a play. Ansible Roles can be imported into a play, reused in other plays or playbooks, and shared for use by other automation users.
* Collections: Bundles of Ansible content designed to help automation developers work faster and improve productivity. Collections are the preferred way to package and share modules.
* Plugins: Pieces of code that build on Ansible’s core functionality and usually execute on the control node. While modules are a type of plugin, they generally execute tasks on a target, not the control node. 
* Modules:  If tasks are the jobs to be completed, then modules are the tools you need to actually complete those jobs. A task defines an action that needs to be done, and the module executes on a managed host to accomplish that action, collecting return values in JSON format when finished. 
Modules are in detail given on page: https://www.redhat.com/en/topics/automation/what-is-an-ansible-module

What is inventory. State it's types
Inventory contains host IP addresses. Either defines in groups or independently. There are 2 types of inventories - Static and dynamic.
Static inventory is the one which we define, where we add the IP addresses manually when new servers are added. 
Dynamic inventory: We write script so that the new servers are automatically detected and added to inventory file.

Is dynamic inventory script already written we can also use plugin- it is already written.
AWS ec2 dynamic inventory config: https://docs.ansible.com/ansible/latest/collections/amazon/aws/aws_ec2_inventory.html#examples

Which ip addresses we e in inventory file, ipv4 or ipv6
-> We use IPv4 address.

Can you explain actual working of Ansible? She explained modules, plugin and playbook
Modules: python codes that executes your defined job or task. They execute tasks on target nodes.
Plugin: Again python code build on Ansible’s core functionality, they run on control node.
Playbook: Set of plays (plays are tasks /list of tasks). Playbook are written in yaml.

What is ansible vault.
-> https://docs.ansible.com/ansible/2.8/user_guide/vault.html
It is a feature of ansible that allows you to keep your sensitive data in encrypted files rather than plain text in playbook or roles.

1. Create a Vault-Encrypted Variables File
Create a file to hold sensitive variables:
bash
ansible-vault create secrets.yml
You’ll be prompted to set a password, then edit the file. Example contents:
text
my_secret_password: supersecret123
This file is now encrypted and unreadable in plaintext.

2. Reference the Vault File in Your Playbook
Example site.yml playbook:
text
---
- name: Example playbook using Vault
  hosts: webservers
  vars_files:
    - secrets.yml  # This file is vault-encrypted
  tasks:
    - name: Show secret password
      debug:
        msg: "Password is {{ my_secret_password }}"

3. Running the Playbook
Run your playbook and provide the vault password for decryption:
bash
ansible-playbook site.yml --ask-vault-pass
Or use a password file:
bash
ansible-playbook site.yml --vault-password-file ~/vault_pass.txt

4. Using Vault in a Role
You can place a secrets.yml file (vault-encrypted) in roles/myrole/vars/ and reference variables in your role’s tasks as usual.

Summary
* Encrypt secrets with ansible-vault create or ansible-vault encrypt.
* Load the encrypted file in your playbook/role using vars_files.
* Run the playbook by providing the vault password or password file.

What is Ansible tower.
Ansible Tower is the commercial version of the Ansible platform, while AWX is the open-source version that was by Red Hat
The biggest advantage of using Ansible AWX over Tower is that it provides you almost the same core functionality as that of Tower but without incurring the expensive enterprise licensing cost of Tower.
Ansible Tower is the commercial version of Ansible, and it provides a web-based user interface, REST API, and other tools that enable the management of automation of IT infrastructure & deployment at scale.
Some of the key features of Ansible Tower include:
- Team collaboration and role-based access controls (RBAC)
- Graphical inventory management and reporting
- Integration with popular systems such as AWS, OpenStack, and VMware

What is AWX?
AWX is the open-source version of Ansible Tower, and it is designed to provide the same functionality as Tower, but without the licensing costs. AWX was created to provide a fully open-source automation platform, and it includes many of the same features as Ansible Tower. 
Good blog on Ansibel tower and AWX: https://blog.devops.dev/ansible-tower-vs-awx-under-the-hood-65cfec78db00

Hoa to copy multiple files form source to destination recursively- we can use module or synchronisation 
To copy multiple files and entire directories recursively in Ansible, use the copy module with a directory as your source.

What does "recursive" mean here?
* Recursive copying means all files, directories, and their subdirectories and contents are copied from the source to the destination.
* For example, if you copy /src/folder/ to /dest/, not just the files inside /src/folder/ but also all subfolders and all their contents are copied to /dest/—mirroring the entire structure.

Example: Copy a Directory Recursively
Suppose on your control host (where Ansible runs), you have this structure:
text
/home/ansible/configs/
  ├── app.conf
  ├── db.conf
  └── templates/
        └── site.template
To copy everything inside configs/ to /etc/myapp/ on a remote host:
text
---
- name: Copy configs recursively to remote host
  hosts: webservers
  tasks:
    - name: Copy directory with all files and subdirectories
      ansible.builtin.copy:
        src: /home/ansible/configs/
        dest: /etc/myapp/
        owner: root
        group: root
        mode: '0644'
* Here, src: ends with / so only the contents (not the top-level directory) are copied into /etc/myapp/.
* The module copies all .conf files and the whole templates directory with its contents.
* The recursive behavior is triggered automatically when your source is a directory.

Alternative: Use recurse flag for selected files
You can use recurse: yes if you need to force recursive permission changes:
text
    - name: Set recursive permissions on destination
      ansible.builtin.file:
        path: /etc/myapp/
        mode: '0644'
        recurse: yes

Summary:
* Use the copy module with a directory as src.
* "Recursive" means Ansible copies everything inside—including all levels of subfolders and files—mirroring the source to the destination, not just a single layer.


How do you use ansible to create encrypted files - there is a command for jt
-> ansible-vault create secret.yaml -> This command will prompt for password -> Provide it and it will encrypt the file.

Difference between play and playbook.
Play is a task or set of tasks while playbook contains list of plays.

Note: Plugin can be used in vs code so it will automatically take care of indentation 

Write playbook to install nginx file

How do you save playbook
With .yaml extension in required folder or repo

ps waux l grep nginx. - to confirm if nginx
Task is done, implemented


1. Setup in ansible galaxy out of which there are few decommissioned servers . How will you exclude or identify the decommissioned ones?  
By using dynamic inventory. As dynamic inventory gathers the IP during the execution of playbook it automatically picks the servers which are running and not the deleted ones. We can use the status flag as state= running to only filter the running servers.

1. **What is Ansible and how does it fit into DevOps?** 
Ansible is an open source automation tool used for configuration management, application deployment, and orchestration, helping DevOps teams achieve consistent and repeatable infrastructure. 
2. **What is a playbook in Ansible?** 
A playbook is a YAML file that defines tasks, roles, and configurations to automate a process on target machines. 
3. **What are Ansible modules?** 
Modules are reusable units in Ansible that perform specific tasks like installing packages, managing files, or starting services. 
4. **What is an inventory file in Ansible?** 
An inventory file lists the hosts or groups of hosts where Ansible will execute tasks. 
5. **How do you define variables in Ansible?** 
Variables in Ansible can be defined in playbooks, inventory files, or separate variable files to make automation dynamic and reusable. 
6. **What is idempotency in Ansible?** 
Idempotency ensures that running a playbook multiple times results in the same system state without unintended side effects. 
7. **What is the difference between a role and a playbook?** 
	A role is a reusable structure containing tasks, files, templates, and variables, whereas a playbook is a sequence of plays executed on hosts. 
8. **How can Ansible be integrated with CI/CD pipelines?** 
Ansible can automate deployments and configuration changes as part of CI/CD pipelines using tools like Jenkins, GitLab CI, or Azure DevOps. 
9. **What is a task in Ansible?** 
A task is a single action executed on a target host, such as installing a package or starting a service. 
10. **How do you handle sensitive data in Ansible?** 
Sensitive data can be handled using Ansible Vault to encrypt secrets and prevent them from being exposed in playbooks.
11. **What is dynamic inventory in Ansible?** 
Dynamic inventory allows Ansible to fetch host details from external sources like cloud providers or CMDBs instead of static files. 
12. **How do you use templates in Ansible?** 
Templates are Jinja2 files used to dynamically generate configuration files based on variables during automation. 
13. **What is a handler in Ansible?** 
A handler is a task triggered only when notified by another task, typically used for restarting services after configuration changes. 
14. **What is Ansible Galaxy?** 
Ansible Galaxy is a repository of community roles and collections that can be reused in your playbooks to save time and effort. 
15. **How do you debug in Ansible Playbook?** 
Debugging can be done using the debug module, verbose mode (`-v`), and checking return codes of tasks to identify issues. 
16. **What is a fact in Ansible?** 
Facts are system information collected by Ansible about hosts, such as OS details, IP addresses, and hardware specs. 
17. **What is the difference between ad-hoc commands and playbooks?** Ad-hoc commands are one-time commands executed from the command line, while playbooks define structured, repeatable automation workflows. 
18. **How do you perform rolling updates with Ansible?** 
Rolling updates are performed using serial execution in playbooks, updating a subset of hosts at a time to ensure minimal downtime. 
19. **What is Ansible Tower?** 
Ansible Tower is a web-based interface for Ansible that provides role-based access, job scheduling, and centralised management of playbooks. 
20. **How do you manage multiple environments in Ansible?** 
Multiple environments can be managed using inventory files, group_vars, host_vars, and environment-specific playbooks. 
21. **What is an idempotent module in Ansible?** 
An idempotent module ensures that applying the module multiple times results in the same state, without changing the system unnecessarily.
22. **What is the difference between static and dynamic inventory?** 
Static inventory uses a fixed list of hosts, while dynamic inventory fetches hosts dynamically from external sources. 
23. **What is a role dependency?** 
A role dependency is when one role requires another role to be executed first, specified in the `meta/main.yml` file. 
24. **What is Ansible Collection?** 
Collections are packaging formats that bundle modules, roles, plugins, and playbooks for easy distribution and reuse. 
25. **How do you manage Ansible versions across teams?** 
Using virtual environments, version control, and `requirements.yml` files to ensure consistent Ansible versions and dependencies. 
26. **What is idempotent configuration management?** 
Idempotent configuration ensures that repeated automation tasks result in the same system state, avoiding unintended changes. 
27. **How do you handle errors in Ansible tasks?** 
Errors can be handled using `ignore_errors`, `failed_when`, or `block-rescue` constructs to manage failures gracefully. 
28. **What is a delegate_to in Ansible?** 
`delegate_to` allows a task to run on a host other than the target host, useful for centralising operations. 
29. **What is a block in Ansible?** 
A block groups multiple tasks together, allowing shared error handling, retries, or conditional execution. 
30. **What is check mode in Ansible?** 
Check mode simulates playbook execution without making changes to the target systems, useful for validation. 
31. **What is ansible-pull?** 
Ansible-pull is a mode where nodes pull configuration from a central repository, suitable for distributed systems. 
32. **How do you manage secrets in Ansible Vault?** 
Secrets can be encrypted, decrypted, and accessed in playbooks using vault passwords or vault id files.
33. **What is a notify in Ansible?** 
Notify triggers a handler when a task reports a change, commonly used to restart services after updates. 
34. **How do you structure a large Ansible project?** 
By organising into roles, playbooks, group_vars, host_vars, and separate environment directories. 
35. **What is async in Ansible?** 
Async allows tasks to run asynchronously with a specified timeout, enabling non-blocking execution. 
36. **What is a loop in Ansible?** 
Loops allow repeating tasks for multiple items, often using `with_items` or `loop` constructs. 
37. **What is a when condition in Ansible?** 
When conditions control task execution based on variables or facts, enabling conditional automation. 
38. **How do you perform rolling restarts in Ansible?** 
By using serial execution and handlers, ensuring minimal downtime while restarting services gradually. 
39. **What is a lookup plugin?** 
Lookup plugins fetch data from external sources, files, or APIs to use in playbooks dynamically. 
40. **What is the difference between delegate_to and run_once?** 
`delegate_to` runs a task on a specific host, while `run_once` executes the task only once regardless of host count. 
41. **What is a template module?** 
The template module renders Jinja2 templates and writes the result to a file on the target host. 
42. **What is a role default variable?** 
Default variables in a role are defined in `defaults/main.yml` and can be overridden by higher precedence variables. 
43. **What is a handler vs task?** 
Handlers are triggered only on change notifications, while tasks execute directly when reached in the playbook.
44. **What is gather_facts?** 
Gather_facts collects system information about hosts, such as OS type, IP addresses, and memory, for use in tasks. 
45. **How do you use tags in Ansible?** 
Tags allow selective execution of tasks or roles, enabling partial playbook runs for efficiency. 
46. **What is a notify handler chain?** 
Handler chains allow multiple handlers to trigger sequentially based on changes in tasks. 
47. **How do you test Ansible playbooks?** 
Testing can be done using `ansible-playbook --check`, molecule framework, or dry run in development environments. 
48. **What is molecule in Ansible?** 
Molecule is a framework for testing Ansible roles in playbooks using Docker, vagrant, or cloud instances. 
49. **What is Ansible.cfg?** 	
Ansible.cfg is the configuration file where settings like inventory location, roles path, and connection options are defined. 
50. **What is a fact cache?** 
Fact cache stores collected facts to avoid repeated collection on multiple playbook runs, improving performance. 
51. **How do you handle loops with complex data?** 
Using `with_items`, `with_dict`, `with_list`, or `loop` constructs along with Jinja2 templating for dynamic iteration. 
52. **What is Ansible-pull vs Ansible-push?** 
Ansible-pull has nodes pull configuration from a repo, while Ansible-push executes playbooks from a control node to target hosts. 
53. **What is the difference between include and import?** 
Include executes tasks dynamically at runtime, while import loads tasks statically before playbook execution. 
54. **How do you schedule Ansible playbooks?** 
Scheduling can be done using cronjobs, Jenkins pipelines, or Tower/AWX job templates. 
55. **What is idempotent deployment?** 
Idempotent deployment ensures repeated automation does not alter system state beyond desired configuration.
56. **What is the difference between hosts: all and hosts: localhost?** 
`hosts: all` targets all hosts in inventory, while `hosts: localhost` runs tasks only on the local machine. 
57. **How do you perform conditional inclusion of tasks?** 
By using `import_tasks` or `include_tasks` with `when` conditions to execute tasks selectively. 
58. **What is the difference between block and include_tasks?** 
Block groups tasks with shared error handling or conditions. Include_tasks dynamically includes external task files. 
59. **How do you version control Ansible projects?** 
Using GIT or other VCS to manage playbooks, roles, and configuration files for collaborative development. 
60. **What is a vault password file?** 
A file containing the password to decrypt Ansible vault secrets during automated playbook execution. 
61. **How do you use loops with nested dictionaries?** 
By iterating over dicts with `with_dict` or `loop` constructs and using Jinja2 templating to access nested keys. 
62. **What is Ansible pull cron?** 
Setting Ansible pull in cron ensures nodes periodically pull and apply configuration updates automatically. 
63. **How do you implement CI/CD with Ansible?** 
By integrating Ansible playbooks into pipelines that automate build, test, and deployment steps across environments. 
64. **What is a rescue block?** 
Rescue blocks handle errors for tasks within a block, allowing recovery actions in case of failure. 
65. **What is an always block?** 
Always blocks run tasks regardless of success or failure of the main block, typically for cleanup or notifications. 
66. **What is the difference between static and dynamic roles?** 
Static roles are predefined in the project. Dynamic roles are pulled from external sources like Ansible Galaxy.
67. **How do you manage configuration drift?** 
By running playbooks regularly to enforce desired state and using monitoring to detect drift. 
68. **What is the difference between `ansible` and `ansible-playbook` commands?** `ansible` runs ad-hoc commands. `ansible-playbook` executes structured playbooks. 
69. **What is a YAML anchor and alias in Ansible?** 
YAML anchors (`&`) and aliases (`*`) allow reuse of variables, tasks, or structures within playbooks for consistency. 
70. **How do you handle multiple OS types?** 
Using conditionals with `ansible_facts` and targeting tasks or roles per OS type in playbooks. 
71. **What is Ansible Lint?** 
Ansible Lint is a tool to check playbooks for syntax, style, and best practises to maintain code quality. 
72. **What is a collection namespace?** 
A namespace organises collections to prevent conflicts, distinguishing community, certified, or custom roles and modules. 
73. **What is serial execution?** 
Serial execution limits the number of hosts processed at a time, useful for rolling updates or staged deployments. 
74. **How do you ensure secure communication with hosts?** 
By using SSH keys, proper permissions, vaults for secrets, and avoiding plain text credentials. 
75. **What is the difference between `local_action` and `delegate_to`?** 
`local_action` runs tasks on the control node. `delegate_to` runs tasks on a different target host. 
76. **What is the difference between template and copy modules?** 
Template renders a file with variables using Jinja2. Copy simply transfers a static file to the target host. 
77. **How do you handle dependencies between roles?** 
By specifying dependencies in `meta/main.yml` to ensure required roles execute in order. 
78. **What is the difference between notify and handler?** 
Notify calls a handler when a task changes. Handlers contain tasks executed only on notification.
79. **What is Ansible pull use case?** 
Useful in distributed environments where each node should pull and apply configuration autonomously. 
80. **How do you implement blue-green deployments with Ansible?** 
By provisioning two environments, blue and green, and switching traffic after deploying and testing the new version. 
81. **What is an ephemeral environment?** 
A temporary environment created for testing or feature deployment, often destroyed after use. 
82. **What is a control node in Ansible?** 
The control node is the machine where Ansible runs commands and manages target hosts. 
83. **What is a managed node in Ansible?** 
Managed nodes are the target machines where tasks and playbooks are executed. 
84. **What is pull-based vs push-based automation?** 
Pull-based nodes fetch configuration from a central repo (Ansible-pull), push-based automation runs from the control node to targets. 
85. **How do you handle secrets in CI/CD pipelines with Ansible?** 
By using Ansible Vault, environment variables, or secret management tools integrated into the pipeline. 
86. **What is an Ansible collection vs role?** 
A collection bundles multiple roles, modules, plugins, and playbooks. A role is a single reusable structure within a collection. 
87. **How do you perform idempotent database migrations?** 
By using tasks that check schema version or state before applying changes to avoid repeated modifications. 
88. **What is the difference between `include_role` and `import_role`?** 
`import_role` loads roles statically before execution. `include_role` loads roles dynamically at runtime. 
89. **What is a dynamic variable in Ansible?** 
A variable whose value is determined at runtime, often based on facts, calculations, or external data. 
90. **How do you optimise large Ansible playbooks?** 
By using roles, handlers, loops, includes, and minimising repetitive tasks for efficiency and readability.
91. **What is Ansible connection plugins?** 
Plugins that define how Ansible connects to target hosts, e.g., SSH, local, WinRM. 
92. **How do you use `with_fileglob` in loops?** 
It loops over a list of files matching a pattern, often used for copying or templating multiple files. 
93. **What is the difference between `force` and `state=present` in copy module?** `force` overrides files unconditionally, `state=present` copies only if file is missing or changed. 
94. **What is Ansible callback plugin?** 
Callback plugins allow custom actions when tasks are executed, such as logging, notifications, or reporting. 
95. **How do you handle high availability with Ansible?** 
By automating deployment across multiple nodes, load balancers, and failover configurations. 
96. **What is the difference between yum and package module?** 
Yum is OS-specific for Red Hat-based systems. Package is generic and abstracts the underlying package manager. 
97. **What is check mode with handlers?** 
Handlers are not executed in check mode unless explicitly forced, preventing unintended changes during dry run. 
98. **How do you implement canary deployment with Ansible?** 
By deploying to a small subset of hosts first, monitoring results, and gradually rolling out to the rest. 
99. **What is a frozen role?** 
A frozen role is a versioned role that does not change, ensuring stable deployments across environments. 
100. **How do you use facts in conditionals?** 
Facts collected by Ansible can be referenced in `when` conditions to execute tasks based on system state.


ubuntu@ip-172-31-18-202:~/.ssh$ cat authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCt3uBu+SQXqwE2akO95M1v0U8TlL3celzJtTG8uyiQKW8d0e7mVyii3IYsseWm3p6sNBv+MdfqvyUnGheWrhlM88CDa5Y2Z9eA38a7U+Nzv1xmt0frA+Pbj8T64zSsXlfkSA2RVOZpv3xAj1Tof/8y7npM+6QFtqi4sYtPfbPYrdkCSMcGN/kaLLUxPazZ0pOZ58t72MUTgsnUL4EnmhSBwWp5uh0DpFQfGzRZ+umGLjopP2KZ/uzJL6zRj1PZHYb7On9T+L2m8preZXEX0A0vmZ0zpGLTQwo6wVFw97lAtdGdy7sU/Izt+O9l+imMfCo6Apqydsx2jen/oTuNp+eX demo-key

MAC ip- 10.84.120.9
Target- 172.31.31.122
Windows - It will be WIN RM for windows and bit different. Hence will keep windows aside for now. I will do windows config if needed in orgs.

Notes to put on git:

Background story for development of Ansible which is a configuration management tool: 
There is a system administrator in an organization who works on on-premises servers. There are 50 Linux machines in total—25 CentOS and 25 Ubuntu. When update needs to be performed for the machines, apply security patches, or install software, either in bulk or on individual machines, they have to use bash or shell scripts to handle it in bulk, but it was still a tedious task because the shell would change based on the operating system, which meant the script had to be updated accordingly.
Now, with the move to the cloud, the number of servers have increased tenfold. Hence, the concept of configuration management was introduced.

What is Ansible? 

Ansible is an open-source configuration management, application deployment, and automation tool that enables DevOps teams to automate provisioning, configuration, and management of infrastructure.

● Agentless (uses SSH) 
● Written in Python (Ansible is written in Python, so you can create your own modules, such as a custom load balancer module, and share them with the community.)
● Manages servers, installs packages, configures infrastructure 

Why Do We Need Ansible? 

● Automates repetitive tasks like package installations, service management, and file configuration. 
● Improves consistency across environments (dev, staging, prod). 
● Reduces human error during deployments and updates. 
● Enables Infrastructure as Code (IaC) to version and manage infrastructure.  ● Useful in provisioning, configuration management, and orchestration. 
● Helps manage large environments from a single control machine 
● Integrates well with cloud platforms like AWS, Azure, and GCP
 Installing Ansible on Ubuntu Server:

To install Ansible on an Ubuntu-based server, use: 
sudo apt update 
sudo apt install ansible -y 
You can verify installation with: 
ansible --version 

Ansible vs Puppet:

In configuration management, there are several tools like Puppet, Chef, Ansible, etc. 
Among these, Ansible has been found useful by many. It is actively developed by the team at Red Hat.


Why Ansible? 

Feature	Puppet	Ansible
Execution Model	Puppet follows a pull model.	Ansible uses a push model.
Patch Deployment	—	When patching needs to be done, the code can be written locally (e.g., on a laptop) and pushed to all servers using an Ansible playbook.
Architecture	Puppet uses a master-slave architecture, where servers need to be configured as agents (or slaves) of the master to receive updates.	Ansible uses an agentless model. It uses SSH for linux and WinRM for windows and password less authentication model.
OS Support	—	Ansible supports both Windows and Linux systems effectively.
Language Used	Puppet requires code to be written in its own domain-specific language (Puppet language).	Ansible uses YAML, which is simpler and more human-readable.


 Limitations of Ansible:
1. It has some limitations when managing Windows servers.
2. Debugging can be difficult—although Ansible has a debug mode, its logs are not very detailed or helpful for troubleshooting.
3. Performance issues may arise when executing tasks in parallel across thousands of servers.


Setting up Ansible:

The prerequisite for using Ansible is passwordless authentication. Ansible must be able to communicate with the servers without needing a password.

How to do it? The steps are as below:

Ansible server = Machine from which we are doing/managing the configuration changes to other machines. 
Target ansible machine/server= Machines on which config changes are to be done. 

From your main Ansible server (for example, an EC2 instance), take the private IP address of the target EC2 instance and try to SSH into it. It will prompt for a password, but we need to eliminate this step because Ansible requires passwordless SSH access.
How do we set this up?:
1. On the Ansible server, go to the path /home/ubuntu/.ssh.
2. Run the command ssh-keygen. This will generate a key pair. A .pub file will be created—either an RSA key or an ED25519 key (e.g., id_rsa.pub or id_ed25519.pub).
3. Copy the contents of the .pub file.
4. SSH into the target server, go to its .ssh directory, and open the authorized_keys file.
5. Paste the public key into the authorized_keys file.
6. Save and close the file.
Now, go back to your Ansible server, use the IP address of the target server, and try to SSH into it again. This time, it should succeed without asking for a password. This is the main setup step.
Even if you have hundreds of servers, you can configure this same public key in the authorized_keys file of each target server.

To set ansible on windows refer online sites/documents.

About Ansible playbook:  Just like you call a shell file a shell script, and a Python file a Python script, in Ansible, a file is called a playbook. So, an Ansible playbook is simply an Ansible file.
Is it mandatory to write a playbook? No. For small tasks or a couple of quick actions, you don’t need to write a playbook. You can use Ansible commands directly in the command line. These are known as Ansible ad-hoc commands. It depends on the nature of the task—you decide accordingly.

Inventory file: 
The next step is to create an inventory file in Ansible to store the IP addresses of the target servers. You can store the inventory file anywhere, as you can always specify its path in the command or in the playbook. However, it is better to store it in the same directory where you plan to execute the command for convenience.
Now, create an inventory file—for example, using the command vi inventory. The file name can be anything; it is not necessary to include the word “inventory” in the name.
In the inventory file, simply mention the IP addresses of your target servers. That’s it—no headers or extra lines are required.

Let’s move on to the Ansible ad-hoc command:

ansible -i <inventory_file_path> <target_ip> -m shell -a "touch devops_class"

Ex.: ansible -i inventory all -m shell -a “touch devops_class”

Let’s break down the command:
* -i refers to the inventory file. Since we are in the same directory where the inventory file is stored, we can directly mention its name. If it’s stored elsewhere, we need to provide the full path.
* <target_ip> is the IP address of the server you want to target. If you want to apply the command to all the IPs listed in your inventory file, you can use ‘all’  instead.
* -m stands for module. Here, we’re using the shell module.
* -a is used to pass the shell command you want to execute. In this example: touch devops_class, which will create a file named devops_class on the target server.
To learn more about Ansible modules, you can search online or visit the official Ansible documentation, where all available modules are listed.

The command above is an Ansible ad-hoc command. When you want to perform one or two quick tasks on the target servers, ad-hoc commands are enough—there’s no need to write a playbook.
Playbooks are generally used when you want to run multiple tasks or perform more complex automation.


Now suppose you have two different sets of IPs—one for DB servers and another for web servers. In that case, you can use grouping in the inventory file.
To do this, use square brackets to define groups. For example:

[db_servers]
172.31.62.68

[web_servers]
172.31.62.100

This is how you structure the inventory file when you have multiple groups of servers.
Now, in the Ansible command, instead of using all, you can specify the group name. This allows you to target only a specific group of servers for certain tasks.
 For example:
ansible -i inventory web_servers -m shell -a "df"
This command will run the df command (to list disk space) only on the servers under the web_servers group.

If you want to perform multiple tasks, then it's better to use a playbook.

Ansible playbook:

Let’s take an example: installing nginx and starting it.
Ansible playbooks are written in .yml files (YAML format).


An Ansible playbook starts with three hyphens (---) at the top. Let’s write an example Ansible playbook:

---
- name: Install and start nginx
  hosts: all
  become: root
  tasks:
    - name: Install nginx
      shell: apt install nginx

    # Alternative way using the apt module:
    # - name: Install nginx
    #   apt:
    #     name: nginx
    #     state: present

    - name: Start nginx
      shell: systemctl start nginx

    # Alternative way using the service module:
    # - name: Start nginx
    #   service:
    #     name: nginx
    #     state: started

—

Explanation:
* A playbook always begins with three hyphens (---).
* name indicates the task or play name.
* A hyphen (-) before name: indicates the start of a task.
* hosts: tells Ansible which group of servers to run the tasks on (in this case, all).
* become: root ensures that commands needing sudo or root access are executed properly.
* You can write tasks using shell commands (shell:) or preferred Ansible modules like apt: and service: or even by using ansible built in modules.
* Indentation and spacing are very important in Ansible. For example, you need to keep a space between the hyphen and the name.
The two different ways of writing the tasks (using shell or using modules like apt and service) do the same thing. However, the second method (using modules) is generally preferred because:
* It is more structured.
* It is idempotent (doesn’t repeat an action if it’s already done).
* It works better with Ansible’s internal mechanisms.

How to create and run the playbook:
1. Create a file using: vi first_playbook.yml
2. To execute the playbook, run: ansible_playbook -i <inventory_file_path> first_playbook.yml 
* ‘ansible’ keyword is used for running ad-hoc commands.
* ‘ansible_playbook’ keyword is used to run playbooks.
Sometimes, a playbook needs to be executed with root or sudo permissions—for example, when running:

sudo apt install nginx

In such cases, use the become: root keyword in the playbook, as shown above or alternative for this is remote_user: root

Finally, to check the status of nginx on your target server, you can SSH into it and run:

sudo systemctl status nginx


—


The command:

ansible-playbook -vvv -i inventory first-playbook.yaml

Here, the -v flag is used for verbosity. It allows you to see logs while the command is executing. The more v’s you use, the more detailed the debug logs become.


Is Ansible really that simple? The answer is both yes and no.
Organizations often use Ansible for advanced automation—such as creating a Kubernetes cluster or provisioning EC2 instances on AWS. For example, if you're creating three EC2 instances, configuring one as the master and two as worker nodes, it could take around 50–60 tasks to fully set up the master and the data plane using a playbook.
To handle such complex setups more efficiently, Ansible provides a feature called roles.

What are Ansible Roles?
Roles are an efficient way to write and organize playbooks. Like in monolithic architecture there used to be one single codebase in which backend, front end and db are integrated and tightly coupled. And in micro service architecture the backend, frontend and databsae entities are divided into multiple services to ease release and so that one service going down doesn’t impact others. Similarly here ansible roles Is created under which all files are present. 
To create a role, you use the command:

ansible_galaxy role init kubernetes

Understanding the command above:
* ansible_galaxy is the tool used to manage roles., used to create role.
* role indicates that you are creating a new role.
* init is the command to initialize it.
* kubernetes is the name of the role.
This command generates a folder structure with predefined directories where you can organize your tasks, handlers, variables, templates, etc.
When you use roles:
* You don't write everything in a single playbook.yaml file.
* Instead, you refer to roles from your playbook and keep related logic separated inside the role directory structure.
* This keeps the playbooks modular, reusable, and clean.
You can choose which role directories to use based on your needs and delete the ones you don't require.

—


Now, let’s explain the folders under roles in Ansible.
1. meta/ This folder is used to write metadata. For example, you can specify the author of the role, describe what the role does, include licensing information, etc. These details are especially helpful when sharing your role with the community or with other team members.
2. defaults/ This folder is used to store default variables. These variables have the lowest priority in Ansible’s variable hierarchy.
3. vars/ and group_vars/ These are also used to store variables. However, variables in these folders have higher priority compared to defaults.
4. tests/ This folder is used to add unit tests to validate your role’s behavior.
5. handlers/ This folder is used to define handlers, which are triggered by tasks. Handlers are typically used to respond to changes, such as restarting a service. You can think of them as a form of exception handling for tasks that may need follow-up actions.
6. tasks/ This is one of the most important folders. Here, you define the tasks that you want Ansible to perform on the target servers. This is the core logic of your role.
7. README.md Just like in GitHub repositories, this file explains how to use the role, what it does, how to run it, and any other relevant documentation.
8. files/ This folder stores static files—such as certificates, an index.html file, or any static resources or links you want to push to the target machine using your role.
9. templates/ This is where you store template files, which use Jinja2 templating. Templates can dynamically render files using variables and logic.


—

Here's the difference between README.md and meta/ (metadata) in the context of Ansible roles:

✅ README.md
* Purpose: Human-readable documentation.
* Used For:
    * Explaining what the role does
    * How to use the role
    * Any dependencies or examples
    * Instructions for setup, usage, and testing
* Audience: Other developers, team members, or open source users.
* Format: Markdown (.md)
* Example content:   # nginx Role
* This role installs and configures nginx on Ubuntu servers.
* 
* ## Usage
* - hosts: web_servers
*   roles:
*     - nginx
*   

✅ meta/main.yml
* Purpose: Structured machine-readable metadata for Ansible and Galaxy.
* Used For:
    * Declaring the author, license, description
    * Defining platforms supported
    * Listing role dependencies
* Audience: Ansible itself (for role validation and dependencies), and package managers like Ansible Galaxy
* Format: YAML
* Example content:   galaxy_info:
*   author: manjiri
*   description: Installs and configures nginx
*   license: MIT
*   platforms:
*     - name: Ubuntu
*       versions:
*         - 20.04
* 
* dependencies: []
*   

🔑 Key Difference:
Feature	README.md	meta/main.yml
Format	Markdown (.md)	YAML
Audience	Humans (users, team members)	Ansible engine and Galaxy system
Purpose	Guide for usage	Metadata & dependency declaration
Required?	Optional (but recommended)	Required for sharing to Ansible Galaxy


The purpose of the vars/ folder in an Ansible role is to define variables that are used within the role, with a higher precedence than the ones defined in the defaults folder.

🔹 Why use vars/?
* To store variables that are essential and should not be overridden easily.
* To organize role-specific configuration in one place.
* To make tasks cleaner by referencing variable names instead of hardcoding values.

🔑 Key Points:
Aspect	vars/
Purpose	Store important role-specific variables
Precedence	Higher than defaults/, lower than extra-vars
Used in	Tasks, handlers, templates, etc.
File format	YAML (main.yml inside vars/ folder)
Overridable?	Not easily overridden from the command line or inventory

🔧 Example:
Folder structure:

roles/
  nginx/
    vars/
      main.yml
vars/main.yml:
yaml
CopyEdit
nginx_port: 80
nginx_user: www-data
tasks/main.yml:
yaml
CopyEdit
- name: Ensure nginx is installed
  apt:
    name: nginx
    state: present

- name: Update nginx port
  template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
templates/nginx.conf.j2:
jinja
CopyEdit
server {
    listen {{ nginx_port }};
    user {{ nginx_user }};
}

🧠 Tip:
Use vars/ for critical or fixed values you don’t expect users to override. If you want to allow overriding, use the defaults/ folder instead.




Interview questions for Ansible:


1. What is Configuration Management? Configuration management is the process of managing systems, software, or hardware in a consistent and automated way. When there are hundreds of systems, and you need to manage tasks like security patching on all of them, manual methods become difficult—especially if the systems run on different operating systems like Windows, Mac, or Ubuntu. You would otherwise need to write different scripts for each OS. Tools like Ansible, Puppet, or Chef (which are configuration management tools) make this process easier and more scalable.

2. Do you think Ansible is better than other configuration management tools? If yes, why? Yes. Here are some advantages of Ansible:
* It is agentless—you don’t need to install any software on the target machines.
* The only prerequisite is passwordless authentication (usually using SSH).
* It is backed by Red Hat, which actively maintains and improves it.
* Many development modules are community-contributed and shared, making it rich in functionality.
* It is written in Python and uses YAML for playbooks, making it easy to learn and write.

3. Can you write an Ansible playbook to install the httpd service and get it running? Yes, this can be done easily using an Ansible playbook.
✅ Tip: Note this example in your notepad and ask the interviewer whether they want you to also check if the firewall is open. 🔸 Why do we need to check the firewall? Because in EC2 instances, you need to enable the security group rules to allow HTTP traffic—just like opening ports in a firewall.

4. How has Ansible helped your organization? Give one practical scenario (don’t overwhelm with too many). Example:
"Previously, we used shell scripting for patching, mandatory installations, and enabling/disabling firewalls across many servers. After adopting Ansible, we automated all of these with playbooks, saving a significant amount of time and reducing manual errors."

5. What is Ansible Dynamic Inventory? Ansible Dynamic Inventory allows Ansible to automatically detect and manage newly added EC2 instances (or other resources) in real time. It dynamically pulls the IPs and details of the instances from cloud providers like AWS. This is useful because you don’t need to manually update your inventory file whenever new servers are added.

6. What is Ansible Tower? Have you used it?
* If you have not used it, be honest: "I haven't used Ansible Tower, but I know that it's the enterprise version of Ansible." 
* If you have used it, you can say: "Ansible Tower provides a graphical user interface (GUI) for Ansible, allowing for easier playbook management, role-based access control, logging, and scheduling." 
Either way, you can add:
"Ansible Tower is backed by Red Hat and provides official support. If you're only using the CLI version of Ansible, that's open-source and community-supported."


7. How do you manage RBAC (Role-Based Access Control) in Ansible Tower? Ansible Tower supports role-based access control (RBAC), which allows you to manage who can do what in the Tower UI. You can assign specific roles and permissions to users or teams, controlling their access to inventories, credentials, projects, templates, etc.

8.  What is the ansible-galaxy command and why is it used? The ansible-galaxy command is used to create or download reusable roles from the Ansible Galaxy community repository. It is commonly used to bootstrap the folder structure for a new role using:
bash
CopyEdit
ansible-galaxy role init <role_name>
This creates all standard directories like tasks/, handlers/, meta/, etc., making the role modular and organized.

9.  Explain the structure of an Ansible playbook using roles. You will have a main playbook YAML file (e.g., site.yaml) in which you reference the role name under the roles: section.
Example:

---
- name: Setup web server
  hosts: web_servers
  roles:
    - nginx
9604569671 - Sangam

Inside the roles/nginx/ directory, you'll have subfolders like:

* tasks/
* handlers/
* files/
* templates/
* vars/
* meta/
Each folder has its specific purpose in organizing your automation logic.

10.  What are handlers in Ansible and why are they used? Handlers are special tasks that only run when notified by another task. They are commonly used for actions like restarting a service after a configuration change.
For example, if a task modifies an nginx config file, it can notify a handler to restart nginx:

- name: Update nginx config
  template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
  notify: Restart nginx

The corresponding handler:


- name: Restart nginx
  service:
    name: nginx
    state: restarted

Handlers are idempotent, and only run once at the end of a play, even if notified multiple times.

11.  I want to run a specific set of tasks only on Windows VMs and not on Linux VMs. Is it possible? Yes, it’s possible. You can do this in several ways:
* Use conditionals (when:) in your tasks based on the OS type.
* Use facts like ansible_os_family or ansible_system.
* Use tags to run selected tasks only.
* Define environment variables or groupings in inventory.

12.  Does Ansible support parallel execution of tasks? Yes, but with a specific behavior:
* Ansible runs each task in parallel across all hosts, and then moves to the next task. For example, Task 1 runs on all servers in parallel, and only after that does Task 2 begin.
* If you want to run all tasks in parallel across all hosts, you would need to run multiple instances of Ansible (manually or via orchestration).
* For deeper parallelism and control, refer to the Ansible documentation on forks and async.

13. What protocol does Ansible use to connect to Windows VMs?
* For Linux machines, Ansible uses the SSH protocol.
* For Windows machines, Ansible uses the WinRM protocol (Windows Remote Management).

14. Can you place the following variable types in order of precedence? (Playbook vars, group_vars, role vars, extra_vars) Yes, Ansible has a well-defined variable precedence order. Out of these, the order is:

extra_vars > role_vars > group_vars > playbook_vars
* extra_vars (passed via command line using -e) have the highest precedence.
* role vars are next—they override group-level variables.
* group_vars are lower in precedence and are applied to a group of hosts.
* Variables defined directly in playbooks (outside roles) typically have lower precedence than extra_vars.

15. How do you handle secrets in Ansible? Ansible comes with a built-in feature called Ansible Vault, which is used to encrypt secrets, passwords, and sensitive variables.
You can encrypt files using:

ansible-vault encrypt secrets.yml

You can also use tools like HashiCorp Vault to manage secrets more securely, and integrate that with Ansible through plugins or lookups.

16. Can we use Ansible for Infrastructure as Code (IaC)? How does it compare to Terraform or other IaC tools? Yes, Ansible can be used for some Infrastructure as Code tasks, like launching EC2 instances, but it's primarily a configuration management tool, not an IaC tool.
Comparison:
Feature	Ansible	Terraform
Primary Purpose	Configuration Management	Infrastructure Provisioning (IaC)
Language	YAML	HCL (HashiCorp Configuration Language)
State Management	No state file	Maintains state
Cloud Resource Support	Limited (via modules)	Extensive
* Ansible is used after resources are created (for patching, installing software, etc.).
* Terraform is used to build and manage infrastructure (like networks, compute, storage, etc.).

17. Can you talk about an Ansible playbook you wrote and how it helped your organization? Prepare 2–3 solid examples of playbooks, including key modules used. Some examples you can mention:
* Installing Oracle Database on multiple nodes with prerequisite configuration.
* Setting up a Kubernetes cluster—installing kubeadm, configuring master and workers.
* Security patching and updates across 100+ servers.
Tip: Refer to Ansible Galaxy for reusable role ideas. Explore GitHub repositories (e.g., by Abhishek Veeramalla) to see professional-level examples and draw inspiration.

18. What do you think Ansible can improve? Some commonly discussed disadvantages or areas of improvement in Ansible include:
* Better verbosity and task-level debug output.
* Improved Windows support, especially for complex automation.
* Support multiple IDEs.
* Improved plugin and extension ecosystem—e.g., better VS Code plugin integration.
* Explore GitHub issues on the official Ansible GitHub page to see real user feedback and known limitations.

19. What is the programming language used to develop Ansible? The answer is Python.
20. Does Ansible support only Windows? No, it supports both Linux and Windows. For Linux, it uses the SSH protocol; for Windows, it uses the WinRM protocol.
21. What is the difference between Puppet and Ansible, or why choose Ansible? We've already discussed this above—Ansible uses a push model and is agentless, among other advantages.
22. Does Ansible use a push or pull model? It uses a push model.
23. What programming language is used to write Ansible playbooks? The answer is YAML.
24. Does Ansible support all cloud platforms? The cloud platform doesn't matter. What matters is that the servers have a private IP address, SSH is enabled, and passwordless authentication is set up for Ansible to work properly.


. Can’t Ansible launch EC2 instances? Then how is it different from IaC tools like Terraform?
Yes, Ansible can launch EC2 instances using modules like amazon.aws.ec2_instance. But the key difference lies in purpose and design:
Feature	Ansible	Terraform (or IaC tools)
Main Focus	Configuration Management	Infrastructure Provisioning
State Management	No built-in state tracking	Maintains state file for tracking
What it does best	Installing software, patching, config files	Creating and destroying resources
When to use	After infrastructure is ready	To provision infrastructure
Example	Install NGINX, configure firewall on EC2	Launch EC2, VPC, Route 53, RDS, etc.
🔹 Summary: Ansible can do some IaC, but it doesn't track state, so it's better suited after the infrastructure exists. Terraform is designed specifically for building that infrastructure in a declarative and trackable way.

🟢 2. Difference between vars and defaults in Ansible Roles
Feature	defaults/	vars/
Precedence	Lowest	Higher
Use case	Allow users to override values easily	Define critical values that shouldn't change
Override by user?	✅ Yes (via extra_vars, inventory, etc.)	❌ Not easily overridden
Typical Use	General-purpose, optional settings	Internal logic values, fixed defaults
🔹 Summary: Use defaults when you want to let users change values. Use vars when you want the role to force a certain value unless it's hardcoded elsewhere.

🟢 3. Short Ansible Role Example with defaults, vars, group_vars, and extra_vars
Let’s say you want to install a web server. Here's a minimal structure:
css
CopyEdit
my-role/
├── defaults/
│   └── main.yml        # Lowest priority
├── vars/
│   └── main.yml        # Medium priority
├── tasks/
│   └── main.yml
defaults/main.yml:
yaml
CopyEdit
web_port: 80
web_package: nginx
vars/main.yml:
yaml
CopyEdit
web_service_state: started
group_vars/webservers.yml:
yaml
CopyEdit
web_port: 8080  # This overrides defaults
Playbook (playbook.yml):
yaml
CopyEdit
- name: Install web server
  hosts: webservers
  roles:
    - my-role
Run with extra vars:
bash
CopyEdit
ansible-playbook playbook.yml -i inventory -e "web_port=9090"
👉 Precedence Order (high to low): extra_vars > vars > group_vars > defaults

🟢 4. What if we need to change YAML tasks based on OS? Do we still need different bash scripts?
No, that’s where Ansible really shines!
You can write conditional tasks using Ansible facts like ansible_os_family, ansible_distribution, or ansible_system.
✅ Example without needing separate scripts:
yaml
CopyEdit
- name: Install web server on Debian
  apt:
    name: nginx
    state: present
  when: ansible_os_family == "Debian"

- name: Install web server on RedHat
  yum:
    name: httpd
    state: present
  when: ansible_os_family == "RedHat"
🔹 Summary: Instead of writing separate bash scripts, you can handle OS-based differences within a single playbook using when conditions.

✅ Recap
* Ansible ≠ IaC, but can do basic provisioning.
* Use defaults/ for overridable values, vars/ for strict internal ones.
* Ansible playbooks adapt to OS conditions—no need for multiple shell scripts.
* Roles keep your playbook clean and reusable.




Ansible server commands:

 1  cat /home/ubuntu/.ssh/id_ed25519.pub 
    2  ssh 172.31.85.30
    3  ssh -i "devop.pem" ubuntu@ec2-44-201-180-99.compute-1.amazonaws.com
    4  ls
    5  adduser user1
    6  sud adduser user1
    7  sudo adduser user1
    8  sudo adduser user2
    9  cat /etc/passwd
   10  su - user1
   11  ssh 172.31.85.30
   12  ssh keygen
   13  ssh-keygen
   14  ls /home/ubuntu/.ssh/
   15  cd /etc/ssh/
   16  sudo vi sshd_config
   17  cd
   18  cd /etc/ssh/
   19  ls
   20  cat ssh_host_rsa_key.pub 
   21  ssh 172.31.81.44
   22  ls
   23  cat ssh_host_ed25519_key.pub 
   24  ssh 172.31.81.44
   25  ls
   26  ssh-keygen
   27  cat /home/ubuntu/.ssh/id_ed25519.pub 
   28  ssh 172.31.81.44
   29  cd
   30  cat /home/ubuntu/.ssh/id_ed25519.pub 
   31  ssh 172.31.90.43
   32  ls
   33  vi inventory
   34  ansibel --version
   35  sudo apt update
   36  sudo apt install software-properties-common
   37  sudo add-apt-repository --yes --update ppa:ansible/ansible
   38  sudo apt install ansible
   39  ansible -i inventory all -m "shell" -a "touch devopsclass"
   40  cat inventory 
   41  vi inventory 
   42  cat inventory 
   43  ansible -i inventory all -m "shell" -a "touch devopsclass"
   ubuntu@ip-172-31-90-60:~$ ansible -i inventory all -m "shell" -a "touch devopsclass"
[WARNING]: Platform linux on host 172.31.90.43 is using the discovered Python interpreter at /usr/bin/python3.12, but future installation of another Python interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-
core/2.18/reference_appendices/interpreter_discovery.html for more information.
172.31.90.43 | CHANGED | rc=0 >>     // Successful


Ansible target server commands:

1  vi /etc/ssh/
    2  cd /etc/ssh/
    3  vi sshd_config
    4  sudo vi sshd_config
    5  sudo service ssh resart
    6  sudo service ssh restart
    7  cd ./ssh
    8  cd
    9  cd ./ssh
   10  cd /home/ubuntu/.ssh/
   11  ls
   12  vi authorized_keys 
   13  history

—

For any commands or modules refer the ansible documentation and ansible module page.

ubuntu@ip-172-31-90-60:~$ cat second_playbook.yml 
---
- name: Install and start nginx
  hosts: all
  become: true

  tasks:
     - name: Install Nginx
       apt:
         name: nginx
         state: present
     - name: Start nginx
       service:
          name: nginx
          state: started

—

ubuntu@ip-172-31-90-60:~$ ansible-playbook -i inventory second_playbook.yml 

PLAY [Install and start nginx] *************************************************************************************************************************************************************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************************************************************************************************************************************************************************
[WARNING]: Platform linux on host 172.31.90.43 is using the discovered Python interpreter at /usr/bin/python3.12, but future installation of another Python interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-
core/2.18/reference_appendices/interpreter_discovery.html for more information.
ok: [172.31.90.43]

TASK [Install Nginx] ***********************************************************************************************************************************************************************************************************************************************************
changed: [172.31.90.43]

TASK [Start nginx] *************************************************************************************************************************************************************************************************************************************************************
ok: [172.31.90.43]

PLAY RECAP *********************************************************************************************************************************************************************************************************************************************************************
172.31.90.43               : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

——

ubuntu@ip-172-31-90-60:~$ ansible-galaxy  role init kubernetes
- Role kubernetes was created successfully
ubuntu@ip-172-31-90-60:~$ cd kubernetes/
ubuntu@ip-172-31-90-60:~/kubernetes$ ls
README.md  defaults  files  handlers  meta  tasks  templates  tests  vars
ubuntu@ip-172-31-90-60:~/kubernetes$ ls -ltr
total 36
drwxrwxr-x 2 ubuntu ubuntu 4096 Jun 18 01:54 templates
drwxrwxr-x 2 ubuntu ubuntu 4096 Jun 18 01:54 files
-rw-rw-r-- 1 ubuntu ubuntu 1328 Jun 18 01:54 README.md
drwxrwxr-x 2 ubuntu ubuntu 4096 Jun 18 01:54 handlers
drwxrwxr-x 2 ubuntu ubuntu 4096 Jun 18 01:54 defaults
drwxrwxr-x 2 ubuntu ubuntu 4096 Jun 18 01:54 tasks
drwxrwxr-x 2 ubuntu ubuntu 4096 Jun 18 01:54 meta
drwxrwxr-x 2 ubuntu ubuntu 4096 Jun 18 01:54 tests
drwxrwxr-x 2 ubuntu ubuntu 4096 Jun 18 01:54 vars


Ansible playbook to install and run httpd:
---
- name: Install and run httpd
  hosts: all
  become: true
    
  -tasks:
    name: Install httpd
    apt:
      name: httpd
      state: present
    name: Run httpd
    service:
      name: httpd
      state: started


=====

Why do we need to ask: "Is the firewall open?"
Because installing and starting httpd is not enough. You also need to make sure that the firewall allows traffic on port 80 (HTTP) (and possibly 443 for HTTPS).

🔧 Example:
You write an Ansible playbook that:
* Installs httpd
* Starts and enables the service
But if firewall (like ufw, firewalld, or AWS security groups) is blocking port 80, then:
* Apache is running ✅
* But no one can access your website ❌
Hence: "Is the firewall open?" becomes an essential question.

🔐 Why does the firewall matter?
A firewall controls network access to/from your server. For httpd, you need:
Port	Protocol	Purpose
80	TCP	HTTP traffic
443	TCP	HTTPS traffic
If these aren't open, browsers will hang or fail trying to connect.

✅ What should you do in your Ansible playbook?
Add a task to open the port in the firewall. For example:
For firewalld (RHEL/CentOS):
yaml
CopyEdit
- name: Open port 80 for HTTP
  firewalld:
    port: 80/tcp
    permanent: true
    state: enabled
    immediate: yes
For ufw (Ubuntu):
yaml
CopyEdit
- name: Allow HTTP through UFW
  ufw:
    rule: allow
    port: '80'
    proto: tcp

✅ Also check AWS EC2 Security Groups
If you're deploying to AWS, you must also:
* Go to EC2 → Security Groups
* Allow Inbound rule for port 80 (HTTP) from 0.0.0.0/0 (or restricted IPs)

🧠 Summary
Task	Why it's needed
Install httpd	To serve web pages
Start httpd	So the service is running
Open firewall	So traffic can reach the web server ✅

Let me know if you'd like to add automatic firewall checks or port verification in your playbook!

You said:
how is httpd different from http and https?

ChatGPT said:
Great question! Let's break it down clearly:

🔍 What’s the difference between HTTP, HTTPS, and HTTPD?
Term	Stands For	What It Is
HTTP	HyperText Transfer Protocol	A protocol used to transfer web data (like HTML, images, etc.) between a browser and a server.
HTTPS	HTTP Secure	Same as HTTP, but with encryption (via SSL/TLS) for secure communication.
HTTPD	HTTP Daemon	A web server program (like Apache) that handles HTTP(S) requests and serves content.

💡 In Simple Terms:
* HTTP / HTTPS → Protocols (rules for web communication)
* HTTPD → The software (a daemon/server) that understands and speaks those protocols

✅ Examples
* When you open http://example.com → You're using HTTP
* When you open https://example.com → You're using HTTPS (encrypted)
* The server running Apache (aka httpd) or Nginx is the program that receives those requests and returns web content

🧠 Analogy:
Think of it like this:
* HTTP/HTTPS = the language of communication
* httpd = the person (web server) who speaks that language

🔐 Why use HTTPS over HTTP?
Feature	HTTP	HTTPS
Encrypted?	❌ No	✅ Yes (SSL/TLS)
Secure?	❌ Not secure	✅ Secure
SEO Friendly?	❌ No	✅ Yes
Always use HTTPS for real websites to protect user data and boost trust.


If we still need to write different tasks in a YAML playbook for different OSes, then what’s the benefit of using Ansible over other config tools?

🔍 Short Answer:
Yes, you sometimes need OS-specific commands, but Ansible still gives you huge benefits over manual work or lower-level tools — and it handles OS-specific logic much more cleanly and powerfully than most alternatives.

✅ Why Ansible is still beneficial:
1. Cross-platform support is built-in
Ansible lets you handle different OSes like this:
yaml
CopyEdit
- name: Install web server
  package:
    name: "{{ web_package }}"
    state: present
  vars:
    web_package: "{{ 'httpd' if ansible_facts['os_family'] == 'RedHat' else 'apache2' }}"
You're still writing one task — but dynamically adjusting for the OS. That's much cleaner than writing shell scripts or handling OS manually in other tools.

2. You don’t have to write scripts from scratch
Without Ansible:
* You’d need separate bash scripts for each OS
* You’d manage files, permissions, users, and services manually
* Debugging would be painful and not repeatable
Ansible gives you:
* Idempotence (runs safely multiple times)
* Clear YAML syntax
* Modules (no need to learn all Linux shell commands)
* Inventory-based targeting
* Role reuse (e.g., use community roles for Nginx, Docker, Kubernetes)

3. Better than most other tools for OS differences
Compared to other config management tools:
Feature	Ansible	Others (e.g. Puppet, Chef)
Simple YAML syntax	✅ Easy to learn	❌ DSL-heavy (Ruby, etc.)
Agentless (via SSH)	✅ Yes	❌ Often require agents
OS-specific logic support	✅ Using when or vars	✅ But more complex sometimes
Community support	✅ Huge	✅ Good (but more tool-specific)

4. Real-world scenario: installing packages
Instead of doing this manually:
bash
CopyEdit
# Ubuntu
sudo apt install apache2

# CentOS
sudo yum install httpd
With Ansible, you can write:
yaml
CopyEdit
- name: Install web server
  package:
    name: "{{ item }}"
    state: present
  loop:
    - "{{ 'apache2' if ansible_facts['os_family'] == 'Debian' else 'httpd' }}"
1 task handles multiple OSes ✨

🧠 So the key takeaway:
Yes, you do adapt to different OSes, but Ansible makes that adaptation clean, repeatable, and scalable — far easier than alternatives or manual scripting.


Excellent set of questions! Let's walk through each of them clearly. You’re getting into the real mechanics of how Ansible works — and understanding this will really strengthen your YAML and playbook writing.

🔹 1. What does package mean here?
yaml
CopyEdit
- name: Install Apache
  package:
    name: "{{ apache_package }}"
    state: present
✅ Explanation:
* package is an Ansible module.
* It's a generic module that uses the appropriate package manager:
    * apt on Ubuntu/Debian
    * yum or dnf on RedHat/CentOS
* It lets you write one task that works across OSes without hardcoding the package manager.
💡 You could also use apt: or yum: if you want to be OS-specific, but package: is portable.

🔹 2. Why are there curly braces and quotes like this?
yaml
CopyEdit
name: "{{ apache_package }}"
✅ Explanation:
This is called a Jinja2 variable expression in Ansible.
* {{ apache_package }} tells Ansible: "substitute this with the actual value of the variable."
* It's wrapped in {{ ... }} to show it's a dynamic value.
* The double quotes "{{ ... }}" are usually required when it's part of a YAML value, especially if the value might include special characters or spaces.

🔹 3. Where is the variable (apache_package) defined?
Right inside the task using vars::
yaml
CopyEdit
vars:
  apache_package: "{{ 'apache2' if ansible_facts['os_family'] == 'Debian' else 'httpd' }}"
This uses a Jinja2 conditional expression:
* If the OS family is Debian, use 'apache2'
* Else, use 'httpd'
Yes — this is a dynamic variable definition, declared inline just for this task.

🔹 4. What values can state have in Ansible?
For the package module, valid state values include:
State	Meaning
present	Install the package if it's not already
absent	Uninstall the package
latest	Ensure the latest version is installed
removed	Alias for absent

🔹 5. When are curly braces ({{ }}) used in Ansible?
Curly braces are used for:
* Variables: {{ var_name }}
* Conditionals/logic: {{ 'apache2' if condition else 'httpd' }}
* Templates inside files (Jinja2 rendering)
Basically: whenever you want to inject a variable or expression into your playbook or template, use {{ ... }}.

✅ Summary Cheat Sheet:
Concept	Example	Meaning
Module	package:	Generic package install
Variable usage	name: "{{ apache_package }}"	Substitute the variable
Variable def	vars: { apache_package: "httpd" }	Define variable
Curly braces	{{ variable_name }}	Templating/variable
State values	present, absent, latest, removed	Package desired state

How do you store secrets in ansible?
—> Ansible Vault is a built-in feature that allows you to encrypt sensitive data files or variables. You can create an encrypted file using ansible-vault create and encrypt sensitive variables using ansible-vault encrypt_string. 
These encrypted files can be version controlled and shared securely among team members. You can decrypt the files using the ansible-vault decrypt command or provide the vault password during playbook execution.

 
What is an Inventory File 
An inventory file is a configuration file that defines the hosts (servers) that Ansible manages. It can be a static file (INI, YAML) or dynamically generated.
How to Create an Inventory File 
1. Create a file, e.g., inventory:  (vi inventory)

[webservers] 
192.168.56.10 
● server: alias or group name 
● ansible_host: IP of the remote machine 
● ansible_user: SSH login user 
● ansible_ssh_private_key_file: path to your SSH private key 
Running Ad-Hoc Command with Ansible 
You can execute one-off commands using ad-hoc syntax: 
ansible -i /home/ubuntu/inventory server -m shell -a "touch /tmp/12mayfile" 
Explanation: 
● -i /home/ubuntu/inventory: Specifies the inventory file 
● server: Target host/group 
● -m shell: Uses the shell module 
● -a: Command to run 
What Are Playbooks in Ansible 
Ansible playbooks are YAML files that define a series of tasks to be executed on remote machines. They allow multi-step orchestration, idempotency, and parameterization. 
Difference Between Ad-Hoc Commands and Playbooks
Feature	Ad-Hoc Command	Playbook
Purpose	One-time, quick tasks	Full automation workflows
Format	Command line	YAML
Reusability	Low	High
Complexity Handling	Limited	Handles roles, variables, 
conditionals

Creating an Ansible Playbook 
Create a file named myplaybook.yaml: 
--- 
- name: install nginx and running nginx 
hosts: all 
become: yes 
tasks: 
- name: updating 
shell: apt update 
- name: installing nginx 
shell: apt install nginx -y 
- name: start nginx 
shell: systemctl start nginx 
- name: creating files 
shell: touch mymayfile 
Running the Ansible Playbook 
Use the command below: 
ansible-playbook -i inventory myplaybook.yaml 
Explanation: 
● -i inventory: Specifies your inventory file 
● myplaybook.yaml: The YAML playbook to execute

——


What is Ansible Role 
An Ansible role is a structured way of organizing Ansible code (tasks, variables, handlers, templates, files, etc.) into reusable components. Roles promote modularity, reusability, and maintainability in Ansible projects. 
Instead of writing large monolithic playbooks, roles allow tasks to be split into logically separated sections, each managing a specific function such as installing a web server, configuring a database, or deploying an application. 
Need for Ansible Role 
● Modular codebase: Cleanly separates functionality by responsibility. 
● Reusable components: Roles can be reused across multiple playbooks or projects. ● Better collaboration: Each team can work on separate roles independently. ● Scalability: Easy to scale automation when infrastructure complexity grows. ● Community sharing: Public roles can be downloaded and reused via Ansible Galaxy. 
Creating a Role Using ansible-galaxy 
Use the ansible-galaxy command-line tool to scaffold a role: 
ansible-galaxy init nginxsetup 
This creates a role structure like: 
nginxsetup/ 
├── defaults/ 
│ └── main.yml 
├── files/ 
├── handlers/ 
│ └── main.yml 
├── meta/ 
│ └── main.yml 
├── tasks/ 
│ └── main.yml
├── templates/ 
├── tests/ 
│ ├── inventory 
│ └── test.yml 
└── vars/ 
└── main.yml 
Folders Created in Ansible Role 

Folder
defaults /
files/
handlers /
meta/

Purpose 
Default variables for the role (lowest precedence). 
Static files to be copied to managed hosts. Handlers that respond to notify events from tasks. 
Metadata info like dependencies and author. 
tasks/ Main logic of the role (series of tasks to execute). 
template s/ 
Jinja2 templates for dynamic file generation. 
vars/ Variables with higher precedence than defaults/. 
Writing a Task in tasks/main.yml Example task to install Nginx and copy a custom file: --- 
- name: update the system 
shell: apt update 
- name: install nginx 
shell: apt install nginx -y 
- name: copy the files 
ansible.builtin.copy 
src: files/demofile1 
dest: /tmp 
owner: root
group: root 
mode: '0644' 
You would place files in the files/ directory of the role.

You can run ANsible roles in 2 ways-

1. Using ansible-playbook command: For eg: For eg site.yml is a playbookthen include roles anew inside the playbook as below-  - hosts: all   become: true   roles:     - webserver  And run it using command: ansible-playbook site.yml 
2. You can also run it without using a playbook using command: ansible-role site.yml 
