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

        stage('Installation Ansible si nécessaire') {
            steps {
                echo "Installation d'Ansible (si non installé)..."
                bat '''
                    wsl -d Ubuntu -- bash -c "command -v ansible-playbook || (sudo apt-get update && sudo apt-get install -y ansible)"
                '''
            }
        }

        stage('Vérification Version Ansible') {
            steps {
                bat '''
                    wsl -d Ubuntu -- bash -c "ansible-playbook --version"
                '''
            }
        }

        stage('Exécution Playbook') {
            steps {
                echo "Exécution du playbook Ansible..."
                bat """
                    wsl -d Ubuntu -- bash -c "cd \$(wslpath '${WORKSPACE}') && ansible-playbook -i inventory/hosts.yml site.yml -vv"
                """
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
