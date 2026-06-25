pipeline {
    agent any
    stages {
        stage('Trivy Scan') {
            steps {
                sh '''
                /usr/local/bin/trivy image \
                  --format json \
                  --output /tmp/trivy-result.json \
                  nginx:latest
                '''
            }
        }
        stage('Convert to CSV') {
            steps {
                sh '''
                python3 -c "
import json, csv
data = json.load(open('/tmp/trivy-result.json'))
with open('/tmp/trivy-result.csv', 'w') as f:
    w = csv.writer(f)
    w.writerow(['Package','Version','Severity','CVE ID','Description'])
    for r in data.get('Results', []):
        for v in r.get('Vulnerabilities', []):
            w.writerow([v.get('PkgName'), v.get('InstalledVersion'), v.get('Severity'), v.get('VulnerabilityID'), v.get('Description','')])
"
                '''
            }
        }
    }
}
