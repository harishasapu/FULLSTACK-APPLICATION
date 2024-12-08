 pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
            git changelog: false, poll: false, url: 'https://github.com/jaiswaladi246/CountryBank.git'
            }
        }
        
        stage('OWASP Dependency Check') {
            steps {
                 dependencyCheck additionalArguments: ' --scan ./ ', odcInstallation: 'DC'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        
        stage('Trivy') {
            steps {
                 sh "trivy fs ."
            }
        }
        
         stage("Docker Compose") {
            steps {
                script{
                    sh '''
                    curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | jq -r .tag_name)/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
                        chmod +x /usr/local/bin/docker-compose '''
                }
            }
        }
        stage('Build & deploy'){
            steps{
                  sh "docker-compose up -d"
            }
        }
    }
}
