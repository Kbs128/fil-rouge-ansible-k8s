pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
        stage('Check WSL & Ansible') {
            steps {
                echo "Vérification de WSL et d'ansible-playbook..."
                sh '''
                wsl --list --verbose
                wsl -e bash -c "which ansible-playbook"
                wsl -e bash -c "ansible-playbook --version"
                '''
            }
        }
        stage('Run Playbook') {
            steps {
                echo "Exécution du playbook Ansible via WSL..."
                sh 'wsl -e bash -c "ansible-playbook ansible/playbook.yaml"'
            }
        }
    }

    post {
        failure {
            echo "Le pipeline a échoué."
        }
        success {
            echo "Le pipeline s'est terminé avec succès."
        }
    }
}
