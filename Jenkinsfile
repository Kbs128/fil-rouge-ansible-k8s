pipeline {
    agent any

    stages {
        stage('Vérification Prérequis') {
            steps {
                sh '''
                if ! wsl.exe --status > /dev/null 2>&1; then
                    echo "WSL n'est pas installé ou ne fonctionne pas"
                    exit 1
                fi
                '''
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
                sh '''
                wsl bash -c "\
                if ! command -v ansible-playbook > /dev/null; then \
                    echo 'Installation d\\'Ansible...' && \
                    sudo apt update && \
                    sudo apt install -y ansible && \
                    echo 'Ansible installé avec succès'; \
                fi && \
                ansible-playbook --version"
                '''
            }
        }

        stage('Exécution Playbook') {
            steps {
                echo "Exécution du playbook Ansible dans WSL..."
                sh '''
                wsl bash -c "cd \$(wslpath 'C:\\Users\\pc\\.jenkins\\workspace\\Ansible-k8s') && \
                ansible-playbook site.yml"
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
