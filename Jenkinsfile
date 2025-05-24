pipeline {
    agent any

    environment {
        ANSIBLE_FORCE_COLOR = 'true'
        PATH = "C:\\WINDOWS\\System32\\wsl.exe;${env.PATH}"
    }

    stages {
        stage('Vérification Prérequis') {
            steps {
                script {
                    try {
                        bat 'wsl.exe --status'
                    } catch (Exception e) {
                        error "WSL n'est pas correctement installé ou configuré"
                    }
                }
            }
        }

        stage('Checkout SCM') {
            steps {
                echo 'Clonage du dépôt...'
                checkout scm
            }
        }

        stage('Configuration WSL & Ansible') {
            steps {
                echo "Installation et vérification d'Ansible dans WSL..."
                bat '''
                    wsl -e bash -c "sudo apt-get update && sudo apt-get install -y ansible"
                '''
                bat '''
                    wsl -e bash -c "ansible --version"
                '''
            }
        }

        stage('Exécution Playbook') {
            steps {
                echo "Exécution du playbook Ansible dans WSL..."
                bat '''
                    wsl -e bash -c "cd /mnt/c/Users/pc/.jenkins/workspace/Ansible-k8s && ansible-playbook -i inventory/hosts.yml site.yml"
                '''
            }
        }
    }

    post {
        failure {
            echo 'Le pipeline a échoué.'
        }
        success {
            echo 'Le pipeline s\'est exécuté avec succès.'
        }
    }
}
