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
                        bat 'wsl -l -v'
                        bat 'wsl --distribution Ubuntu --status'
                    } catch (Exception e) {
                        error "Ubuntu WSL n'est pas correctement installé"
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
                    wsl --distribution Ubuntu -e bash -c "sudo apt-get update && sudo apt-get install -y ansible"
                '''
                bat '''
                    wsl --distribution Ubuntu -e bash -c "ansible --version"
                '''
            }
        }

        stage('Exécution Playbook') {
            steps {
                echo "Exécution du playbook Ansible dans WSL..."
                bat '''
                    wsl --distribution Ubuntu -e bash -c "cd /mnt/c/Users/pc/.jenkins/workspace/Ansible-k8s && ansible-playbook -i inventory/hosts.yml site.yml -vv"
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
