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
                    script {
                        def scannerHome = tool 'sonar-scanner'
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
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
