pipeline {
    agent any

    stages {
        stage('Cloner depuis GitHub') {
            steps {
                echo '🔄 Clonage du dépôt...'
                checkout scm
            }
        }

        stage('Exécuter le Playbook Ansible') {
            steps {
                echo '🚀 Exécution du playbook Ansible...'
                bat '''
                    wsl -d Ubuntu -- bash -c "
                    cd /mnt/c/Users/pc/.jenkins/workspace/Ansible-k8s && \
                    ansible-playbook -i inventory/hosts.yml playbook.yml
                    "
                '''
            }
        }
    }

    post {
        failure {
            echo '❌ Le pipeline a échoué.'
        }
        success {
            echo '✅ Le pipeline a réussi.'
        }
    }
}
