pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage ('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], 
                userRemoteConfigs: [[url: 'https://github.com/ronnie-cunanan/simple-ansible-jenkins-test.git']])
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                sh """
                docker run --rm \
                  -v \$PWD:/workspace \
                  -v /var/jenkins_home/.ssh:/root/.ssh \
                  -w /workspace \
                  willhallonline/ansible:latest \
                  ansible-playbook playbook.yml -i inventory.ini \
                    --ssh-extra-args='-o StrictHostKeyChecking=no'
                """
            }
        }
    }

    post {
        success {
            echo "Deployment completed successfully"
        }
        failure {
            echo "Deployment failed"
        }
    }
}
