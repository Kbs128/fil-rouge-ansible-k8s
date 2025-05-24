pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/Kbs128/fil-rouge-ansible-k8s.git'
    }

    stages {
        stage('Cloner le dépôt') {
            steps {
                git url: "${GIT_REPO}", branch: 'main'
            }
        }

        stage('Déployer sur Kubernetes avec Ansible') {
            steps {
                ansiblePlaybook playbook: 'ansible/playbook.yml'
            }
        }
    }
}
