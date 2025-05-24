pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                echo 'Clonage du dépôt...'
                checkout scm
            }
        }

        stage('Check WSL & Ansible') {
            steps {
                echo "Vérification de WSL et d'ansible-playbook..."
                sh '''
                wsl bash -c '
                echo "Utilisateur courant : $(whoami)"
                echo "Chemin d\'Ansible : $(which ansible-playbook)"
                if ! command -v ansible-playbook > /dev/null; then
                  echo "Ansible non trouvé"
                  exit 1
                fi
                ansible-playbook --version
                '
                '''
            }
        }

        stage('Run Playbook') {
            steps {
                echo "Exécution du playbook Ansible dans WSL..."
                sh '''
                wsl bash -c '
                cd /mnt/c/Users/pc/.jenkins/workspace/Ansible-k8s
                ansible-playbook site.yml
                '
                '''
            }
        }
    }

    post {
        failure {
            echo 'Le pipeline a échoué.'
        }
        success {
            echo 'Pipeline exécuté avec succès.'
        }
    }
}
