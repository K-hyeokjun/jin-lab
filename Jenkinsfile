pipeline {
    agent any
    stages {
        stage('Trivy Scan') {
            steps {
                sh '''
                DATE=$(date +%Y%m%d)
                /usr/local/bin/trivy image \
                  --format template \
                  --template "@/usr/local/bin/trivy-contrib/csv.tpl" \
                  --output /tmp/trivy_${DATE}.csv \
                  nginx:latest
                '''
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'trivy_*.csv', allowEmptyArchive: true
        }
    }
}
