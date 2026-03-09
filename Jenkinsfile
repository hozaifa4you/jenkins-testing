pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage("SonarQube Analysis"){
            step("Run SonarQube Scanner") {
                withSonarQubeEnv('devops sonarqube') {
                    sh 'sonar-scanner -Dsonar.projectKey=devops-practice -Dsonar.sources=. -Dsonar.host.url=http://34.47.206.47:9000 -Dsonar.token=sqp_2ff8e803140c0b7bdf648c45942b89a80ab3dd19'
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
