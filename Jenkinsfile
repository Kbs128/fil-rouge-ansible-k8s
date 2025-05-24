pipeline {
    agent any
    stages {
        stage('Check Ansible') {
            steps {
                echo 'Vérification de la présence d\'ansible-playbook dans WSL...'
                sh 'wsl which ansible-playbook'
            }
        }
        stage('Check Playbook') {
            steps {
                echo 'Liste du playbook pour vérifier son existence...'
                sh 'wsl ls -l /mnt/c/Users/pc/.jenkins/workspace/Ansible-k8s/ansible/playbook.yml'
            }
        }
        stage('Run Playbook') {
            steps {
                echo 'Exécution du playbook Ansible via WSL...'
                sh 'wsl ansible-playbook /mnt/c/Users/pc/.jenkins/workspace/Ansible-k8s/ansible/playbook.yml'
            }
        }
    }
    post {
        success {
            echo 'Playbook exécuté avec succès !'
        }
        failure {
            echo 'Échec lors de l\'exécution du playbook.'
        }
    }
}
