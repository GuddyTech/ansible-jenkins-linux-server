# ansible-jenkins-linux-server
This is a project that uses Ansible to stop, start and restart a Springboot Application in a server from the Jenkinsfile when the app(app1), env(dev) and action(start) is specified

Start,stop and restart a service with ansible and jenkins on a linux server
mkdir -p ansible/playbooks
cd ansible/playbooks/
Create a file hosts.ini   this is the inventory file for the environment servers:
[dev]
host1 ansible_host=192.168.1.10 ansible_user=username ansible_ssh_private_key_file=/path/to/private_key
host2 ansible_host=192.168.1.11 ansible_user=username ansible_ssh_private_key_file=/path/to/private_key

[uav]
host1 ansible_host=192.168.1.10 ansible_user=username ansible_ssh_private_key_file=/path/to/private_key
host2 ansible_host=192.168.1.11 ansible_user=username ansible_ssh_private_key_file=/path/to/private_key

[prod]
host1 ansible_host=192.168.1.10 ansible_user=username ansible_ssh_private_key_file=/path/to/private_key
host2 ansible_host=192.168.1.11 ansible_user=username ansible_ssh_private_key_file=/path/to/private_key


Create a start.yml file: 
---
- name: Start SpringBoot Application
  hosts: "{{ env }}"
  become: yes
  tasks:
    - name: Start application
      systemd:
        name: "{{ application_name }}"
        state: started


Create a stop.yml file: 
---
- name: Stop SpringBoot Application
  hosts: "{{ env }}"
  become: yes
  tasks:
    - name: Stop application
      systemd:
        name: "{{ application_name }}"
        state: stopped


Create a restart.yml file: 
---
- name: Restart SpringBoot Application
  hosts: "{{ env }}"
  become: yes
  tasks:
    - name: Restart application
      systemd:
        name: "{{ application_name }}"
        state: restarted


Create Jenkins job
Add your parameters:
pipeline {
    agent any
    
    parameters {
        choice(name: 'APPLICATION_NAME', choices: ['app1', 'app2', 'app3'], description: 'Select the SpringBoot application to manage')
        choice(name: 'ACTION', choices: ['start', 'stop', 'restart'], description: 'Select the action to perform on the application')
    }
    
    stages {
        stage('Manage SpringBoot Application') {
            steps {
                script {
                    def env = 'dev' // Change this to 'uat' or 'prod' as needed
                    def ansibleCommand = "ansible-playbook -i /path/to/ansible/hosts /path/to/ansible/playbooks/${params.ACTION}.yml -e \"env=${env} application_name=${params.APPLICATION_NAME}.service\""
                    sh ansibleCommand
                }
            }
        }
    }
}


This Jenkinsfile defines a pipeline that executes the Ansible playbook based on the selected application and action. The environment is hardcoded to 'dev' in this example, but you can modify it as needed for 'uat' or 'prod'.
as

To do this on the terminal with a server in AWS
Create an EC2 instance on AWS and copy the public ip
Download the private key and know the path/to/private-key.pem
mkdir ansible/playbooks/  in local computer
cd ansible/playbooks/
Create the hosts.ini, start.yml, stop.yml and restart.yml
In the hosts.ini:        
[dev]
host1 ansible_host=54.82.8.130 ansible_user=ubuntu ansible_ssh_private_key_file=/home/guddytech/Pictures/dev.pem

The ansible_host is the public ip of the instance and ansible_user is the username of the instance. Ansible_ssh_private_key_file is the path to the private key of the instance on your local machine
Go to remote server and cd into /etc/systemd/System
Vim myapp.service:
[Unit]
Description=My Spring Boot Application
After=network.target

[Service]
User=springboot
ExecStart=/usr/bin/java -jar /path/to/myapp.jar
SuccessExitStatus=143
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target

Come back to local computer
Sudo apt install ansible-core………..To install ansible
ansible-playbook /path/to/ansible/playbooks/${ACTION}.yml -e "env=dev application_name=${APPLICATION_NAME}.service"
Export APPLICATION_NAME=myapp
echo $APPLICATION_NAME
sudo ansible-playbook start.yml -i hosts.ini -e "env=dev application_name=${APPLICATION_NAME}.service" -vvv   for debugging
sudo ansible-playbook start.yml -i hosts.ini -e "env=dev application_name=${APPLICATION_NAME}.service" 
Select the env, action and app that you want to run it in the playbook 

