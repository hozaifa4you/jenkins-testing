pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Backend Deploy') {
            when { branch 'main' }
            steps {
                dir('backend') {
                    sh 'chmod +x deploy.sh'
                    sh './deploy.sh'
                }
            }
        }

        stage('Frontend Deploy') {
            when { branch 'main' }
            steps {
                dir('frontend') {
                    sh 'chmod +x deploy.sh'
                    sh './deploy.sh'
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment completed successfully!'
        }
        failure {
            echo '❌ Deployment failed! Frontend skipped if backend fails.'
        }
    }
}
