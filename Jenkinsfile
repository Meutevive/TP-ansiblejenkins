pipeline {
    agent any

    environment {
        ANSIBLE_CONFIG = "${WORKSPACE}/ansible.cfg"
    }

    stages {
        stage('Préparation') {
            steps {
                echo 'Préparation de l’environnement Ansible'
                sh 'ansible --version'
            }
        }

        stage('Déploiement Docker Services') {
            steps {
                echo 'Déploiement des services Docker (Nginx Proxy, WordPress, Vaultwarden, Portainer)'
                sh 'ansible-playbook -i inventory.ini playbook-docker.yml'
            }
        }

        stage('Déploiement Apache Services') {
            steps {
                echo 'Déploiement des services Apache (FileGator, FreshRSS, GLPI, Nextcloud)'
                sh 'ansible-playbook -i inventory.ini playbook-apache.yml'
            }
        }

        stage('Vérification des Services') {
            steps {
                echo 'Vérification de l’état des conteneurs'
                sh 'ansible -i inventory.ini all -m shell -a "docker ps"'
            }
        }
    }

    post {
        always {
            echo 'Pipeline terminé'
        }
        success {
            echo 'Pipeline exécuté avec succès ✅'
        }
        failure {
            echo 'Pipeline échoué ❌'
        }
    }
}
