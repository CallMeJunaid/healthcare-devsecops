pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/CallMeJunaid/healthcare-devsecops.git'
            }
        }

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

                    if ($html -match '<script\\b') {
                        Write-Host "Review inline JavaScript for security issues."
                    }

                    if ($html -match 'password\\s*=') {
                        throw "Potential hard-coded password found."
                    }

                    Write-Host "Basic security checks completed."
                '''
            }
        }
    }

    post {
        success {
            echo 'Healthcare DevSecOps pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check the console output.'
        }
    }
}
