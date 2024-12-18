pipeline {
    agent any

    stages {
        stage ('Install Dependencies') {
            steps {
                echo "Install Dependencies as mentioned in package.json"
                powershell 'npm install --no-audit'
            }
        }
        stage ("dependency Scanning") {
            parallel {
                stage ("NPM Dependency Audit") {
                    steps {
                        echo "NPM Dependency scanning and check for Critical dependency"
                        powershell 'npm audit --audit-level=critical'
                        powershell 'echo $?'
                    }
                }
                stage ('OWASP Dependency Check') {
                    steps {
                        echo "OWASP Dependency check"
                        dependencyCheck additionalArguments: '''
                            --scan \'./\'
                            --out \'./\'
                            --format \'ALL\'
                            --prettyPrint''',  nvdCredentialsId: 'NVD-API-Key', odcInstallation: 'OWASP-DepCheck-11'

                        //Fail if critical dependency found
                        dependencyCheckPublisher failedTotalCritical: 1, pattern: 'dependency-check-report.xml', stopBuild: true

                        //Publish HTML Report
                        publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, keepAll: false, reportDir: 'dependency-check-report.html', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                    }
                }
            }
        }
        stage ('Unit Testing') {
            steps {
                powershell 'npm test'
            }
        }
    }
}