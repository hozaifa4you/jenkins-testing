stage('Run Backend Deploy') {
    when {
        branch 'main'
    }
    steps {
        dir('backend') {
            sh 'chmod +x deploy.sh'
            sh './deploy.sh'
        }
    }
}

stage('Run Frontend Deploy') {
    when {
        branch 'main'
    }
    steps {
        dir('frontend') {
            sh 'chmod +x deploy.sh'
            sh './deploy.sh'
        }
    }
}
