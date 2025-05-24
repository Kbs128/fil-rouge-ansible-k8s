pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                echo '📥 Récupération du code depuis GitHub...'
                checkout scm
            }
        }
        stage('Run Ansible Playbook') {
            steps {
                echo '🚀 Exécution du playbook Ansible...'
                // On utilise WSL pour lancer ansible sous Ubuntu avec le bon chemin
                bat 'wsl -d Ubuntu -- bash -c "cd /mnt/c/Users/pc/.jenkins/workspace/Ansible-k8s/ansible && ansible-playbook -i hosts.yml playbook.yml"'
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline terminé avec succès !'
        }
        failure {
            echo '❌ Échec du pipeline.'
        }
    }
}
