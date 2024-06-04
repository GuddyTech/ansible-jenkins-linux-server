pipeline {
    agent any
    
    parameters {
        choice(name: 'APPLICATION_NAME', choices: ['myapp', 'app2', 'app3'], description: 'Select the SpringBoot application to manage')
        choice(name: 'ACTION', choices: ['start', 'stop', 'restart'], description: 'Select the action to perform on the application')
    }
   
    stages {
        stage('Manage SpringBoot Application') {
            steps {
                script {
                    //def env = 'dev' // Change this to 'uat' or 'prod' as needed
                    
                    //sudo ansible-playbook start.yml -i hosts.ini -e "env=dev application_name=${APPLICATION_NAME}.service" -vvv
                     
                    //def ansibleCommand = "ansible-playbook ansible/playbooks/${params.ACTION}.yml -i ansible/playbooks/hosts.ini -e \"env=${env} application_name=${params.APPLICATION_NAME}.service\""
                    //sh ansibleCommand 
                    sh 'ansible-playbook ansible/playbooks/start.yml -i ansible/playbooks/hosts.ini -e "env=dev application_name=${APPLICATION_NAME}.service" -vvv'
                }
            }
        }
    }
}
