## Ansible_beginner
First time using Ansible- testing out features! Each exercise builds on the last.

Before beginning, I set up 5 EC2 instances in AWS: controlmachine, web01, web02, web03, and db01. Controlmachine & web03 are Ubuntu, the rest are CentOS.
Additionally, I copied my key file onto the controlmachine and just kept it local in each exercises folder.
Lastly before beginning, in the control machine's /etc/ansible/ansible.cfg I turned host_key_checking to false to make seamless, non-interactive connections.

## Exercise 1
Starting simple, I set up an inventory with just web01 and ran an ad hoc command: 'ansible web01 -m ping -i inventory' and got my first successful 'pong' back!

## Exercise 2
Then, I added web02 and db01 to the inventory as well as add groups. I made a group for webservers(web01+web02, dbservers(db01), as well as dc_virginia(group of groups- webservers+dbservers). I then tested pinging various connections.

## Exercise 3
I began my introduction to variables with group variables, adding 'ansible_user' and 'ansible_ssh_private_key_file' for dc_virginia, which significantly reduced a lot of lines of the file as I don't have to define them for every single host.

## Exercise 4
I explored with more ad hoc commands and getting use to Ansible's idempotent nature and how it maintains state- so cool!! 
In specific, I ran 'ansible webservers -m ansible.builtin.copy -a "src=index.html dest=/var/www/html/index.html" -i inventory --become' to copy a file from control machine to the webservers, and viewed the changes in the file via webservers IP addresses.

## Exercise 5
Diving into playbooks, I gave 2 plays with 2 tasks each, utilizing modules 'ansible.builtin.yum' to install httpd & mariadb as well as 'ansible.builtin.service' to start each service. I ran it with 'ansible-playbook -i inventory web-db.yaml', still specifying inventory as I still hadn't yet set up a local ansible.cfg file to mention the inventory path.

## Exercise 6
Similar to what I did in exercise 4 where I copied a file from control machine to webservers, this time I did it via a playbook with 'copy' module.

## Exercise 7
Focusing on the database, after installing + starting Mariadb + installing its dependencies, I created a database named 'accounts' as well as a user 'andrea' with a very super, hard, complex, secure password of '12345'.
I then began focusing on creating an 'ansible.cfg' file for the current directory, and creating a habit to have it in every directory forward.

## Exercise 8
Moving onto variables, I added variables into the playbook. Additionally, I used the module 'debug' to print a message as well as adding the 'register' module with 'debug' to print the output from a task.

## Exercise 9
I then pivoted to using variables outside of the playbook; inventory-based variables. I used group_vars/all, group_vars/webservers, and host_vars/web02. I witnessed the level of priority for variables is playbook, then host_vars, then group_vars/<group>, then finally group_vars/all

## Exercise 10
Focusing on fact variables, I practiced printing certain facts, turning it off, and running it via ad hoc 'ansible -m setup web01'. 
In addition, I didnt forget about web03! I finally added it into inventory, overriding the 'ansible_user' global variable with host variable since it is Ubuntu and has a different default user.

## Exercise 11
Moving onto server provisioning, I used NTP just an example. In this exercise, I . I used 'when' to set a condition on the tasks for the fact variable 'ansible_distribution', so it runs dependent on whether it is Ubuntu and CentOS.

## Exercise 12
Before moving on with server provisioning, I practiced loops- installing multiple packages with one task.

## Exercise 13
Back to provisioning, I started simple with changing the banner file on all to show at login with 'copy' module.
For NTP: I logged into a CentOS server and copied /etc/chrony.conf manually onto control machine in templates/ntpconf_centos. I did the same with Ubuntu.
I updated the playbook to deploy ntp agent conf on the servers with 'template' module, then to also restart the servers. Additionally, I updated the group_vars/all and templates files respectfully with ntp agent servers.

The main point was to see the difference between 'copy' and 'template' modules, copy takes the file and directly dumps at the location, whereas the template module is intelligent. It will find the template, see that there are variables, it will then find the variables values, change it on a new file template, then send that file to client machine.

## Exercise 14
In the last exercise, there was an issue of having the servers restart every single time, regardless of whether there was a change or not. So, I used handlers to make sure the service is only restarted when the configuration has changed.

## Exercise 15
Last but not least, I delved into roles. After creating the role 'post-install'- I copied over tasks, handlers, variables, files, and templates to have a very clean provisioning.yaml file. And that's it!



<img width="1263" alt="Screenshot 2024-01-25 at 1 49 49 AM" src="https://github.com/andreapeterson/ansible_beginner/assets/134665743/7251b3bc-f1cb-4aea-b63b-4aeb4dab09f0">




