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
                            --prettyPrint''', odcInstallation: 'OWASP-DepCheck-11'
                    }
                }
            }
        }
    }
}