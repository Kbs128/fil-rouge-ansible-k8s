pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
        stage('Check Ansible') {
            steps {
                echo "Vérification de la présence d'ansible-playbook dans WSL..."
                sh 'wsl which ansible-playbook'
            }
        }
        stage('Run Playbook') {
            steps {
                script {
                    try {
                        echo "Exécution du playbook Ansible..."
                        sh 'wsl ansible-playbook ansible/playbook.yaml'
                    } catch (err) {
                        echo "Échec lors de l'exécution du playbook."
                        error("Playbook failed: ${err}")
                    }
                }
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
