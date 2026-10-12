pipeline {
    agent any

    stages {
        stage('Check Project Files') {
            steps {
                powershell '''
                    if (!(Test-Path "index.html")) {
                        throw "index.html is missing"
                    }

                    if (!(Test-Path "style.css")) {
                        throw "style.css is missing"
                    }

                    Write-Host "Healthcare project files verified."
                '''
            }
        }

        stage('Basic Security Check') {
            steps {
                powershell '''
                    $html = Get-Content "index.html" -Raw

                    if ($html -match 'password\\s*=') {
                        throw "Potential hard-coded password found."
                    }

                    Write-Host "Basic security checks completed."
                '''
            }
        }

        stage('OWASP ZAP Scan') {
            steps {
                powershell '''
                    docker run --rm `
                      ghcr.io/zaproxy/zaproxy:stable `
                      zap-baseline.py `
                      -t http://host.docker.internal:8000
                '''
            }
        }
    }

    post {
        success {
            echo 'Healthcare DevSecOps pipeline completed.'
        }
        failure {
            echo 'Pipeline failed. Check the Console Output.'
        }
    }
}