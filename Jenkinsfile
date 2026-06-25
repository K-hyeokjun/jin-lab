pipeline {
    agent any
    stages {
        stage('Trivy Scan') {
            steps {
                sh '''
                DATE=$(date +%Y%m%d)
                mkdir -p /tmp/trivy-contrib
                cat << 'TMPL' > /tmp/trivy-contrib/csv.tpl
PackageName,Version,Severity,CVE_ID,Description
{{- range .}}
{{- range .Vulnerabilities}}
{{.PkgName}},{{.InstalledVersion}},{{.Severity}},{{.VulnerabilityID}},"{{.Description}}"
{{- end}}
{{- end}}
TMPL
                /usr/local/bin/trivy image \
                  --format template \
                  --template "@/tmp/trivy-contrib/csv.tpl" \
                  --output /tmp/trivy_${DATE}.csv \
                  nginx:latest
                cp /tmp/trivy_${DATE}.csv ${WORKSPACE}/trivy_${DATE}.csv
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
