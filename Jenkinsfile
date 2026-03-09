pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage("SonarQube Analysis"){
            steps {
                withSonarQubeEnv('devops sonarqube') {
                    sh '''
                        if ! command -v sonar-scanner &> /dev/null; then
                            export SONAR_SCANNER_VERSION=6.2.1.4610
                            curl -sSLo /tmp/sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-${SONAR_SCANNER_VERSION}-linux-x64.zip
                            unzip -q /tmp/sonar-scanner.zip -d /tmp/
                            export PATH="/tmp/sonar-scanner-${SONAR_SCANNER_VERSION}-linux-x64/bin:$PATH"
                        fi
                        sonar-scanner
                    '''
                }
            }
        }

        stage('Deploy') {
            when { branch 'main' }
            parallel {
                stage('Backend') {
                    steps {
                        dir('backend') {
                            sh 'chmod +x deploy.sh && ./deploy.sh'
                        }
                    }
                }
                stage('Frontend') {
                    steps {
                        dir('frontend') {
                            sh 'chmod +x deploy.sh && ./deploy.sh'
                        }
                    }
                }
            }
        }
    }

    post {
        success { echo 'Deployment completed successfully!' }
        failure  { echo 'Deployment failed!' }
    }
}
