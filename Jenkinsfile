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
