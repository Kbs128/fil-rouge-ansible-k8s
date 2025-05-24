pipeline {
    agent any

    environment {
        WSL_DISTRO = 'Ubuntu'
        WORKSPACE_WSL = ''
    }

    stages {
        stage('Vérification Prérequis') {
            steps {
                bat 'wsl -l -v'
                bat "wsl --distribution ${WSL_DISTRO} --status"
            }
        }

        stage('Détermination du chemin WSL') {
            steps {
                script {
                    def path = bat(script: 'wsl -d Ubuntu -- bash -c "wslpath \\"${WORKSPACE}\\""', returnStdout: true).trim()
                    env.WORKSPACE_WSL = path.split('\n')[-1].trim()
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
                echo 'Installation d\'Ansible (si non installé)...'
                bat "wsl -d ${WSL_DISTRO} -- bash -c \"command -v ansible-playbook || (sudo apt-get update && sudo apt-get install -y ansible)\""
            }
        }

        stage('Vérification Version Ansible') {
            steps {
                bat "wsl -d ${WSL_DISTRO} -- bash -c \"ansible-playbook --version\""
            }
        }

        stage('Debug: Vérification des fichiers') {
            steps {
                bat """
                    wsl -d ${WSL_DISTRO} -- bash -c "cd '${env.WORKSPACE_WSL}' && ls -la && cat inventory/hosts.yml && cat site.yml"
                """
            }
        }

        stage('Exécution Playbook') {
            steps {
                echo 'Exécution du playbook Ansible...'
                bat """
                    wsl -d ${WSL_DISTRO} -- bash -c "cd '${env.WORKSPACE_WSL}' && ansible-playbook -i inventory/hosts.yml site.yml -vv || echo ERREUR_PLAYBOOK"
                """
            }
        }
    }

    post {
        success {
            echo 'Le pipeline a réussi.'
        }
        failure {
            echo 'Le pipeline a échoué.'
        }
    }
}
