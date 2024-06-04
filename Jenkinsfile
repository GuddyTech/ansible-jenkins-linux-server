pipeline {
    agent any

    parameters {
        choice(name: 'APPLICATION_NAME', choices: ['myapp', 'app2', 'app3'], description: 'Select the SpringBoot application to manage')
        choice(name: 'ACTION', choices: ['start', 'stop', 'restart'], description: 'Select the action to perform on the application')
    }

    stages {
        stage('Prepare SSH') {
            steps {
                script {
                    def remoteHost = "44.222.204.243"
                    
                    // Create .ssh directory if it doesn't exist
                    //sh 'mkdir -p /var/lib/jenkins/.ssh'
                   // sh 'chown jenkins:jenkins /var/lib/jenkins/.ssh'
                    //sh 'chmod 700 /var/lib/jenkins/.ssh'

                    // Add host key to known_hosts
                    //sh "ssh-keyscan -H ${remoteHost} >> /var/lib/jenkins/.ssh/known_hosts"
                    sh 'hi'
                }
            }
        }
        stage('Manage SpringBoot Application') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'dev-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]) {
                    script {
                        def ansibleCommand = "ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook ansible/playbooks/${params.ACTION}.yml -i ansible/playbooks/hosts.ini -e \"env=dev application_name=${params.APPLICATION_NAME}.service\" --private-key ${keyfile} -vvv"
                        sh ansibleCommand
                    }
                }
            }
        }
    }
}
