pipeline {
    agent any

    environment {
        WSL_DISTRO = 'Ubuntu'
    }

    stages {
        stage('Cloner depuis GitHub') {
            steps {
                echo 'Clonage du dépôt...'
                checkout scm
            }
        }

        stage('Exécuter le Playbook Ansible') {
            steps {
                echo 'Exécution du playbook Ansible...'
                bat """
                    wsl -d ${WSL_DISTRO} -- bash -c "cd /mnt/c/Users/pc/.jenkins/workspace/Ansible-k8s && ansible-playbook -i inventory/hosts.yml site.yml"
                """
            }
        }
    }

    post {
        success {
            echo '✅ Succès : le pipeline a réussi.'
        }
        failure {
            echo '❌ Échec : le pipeline a échoué.'
        }
    }
}
