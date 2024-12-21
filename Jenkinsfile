pipeline {
    agent any
    environment {
        MONGO_URI = "mongodb+srv://supercluster.d83jj.mongodb.net/superData"
        SONAR_SCANNER_HOME = tool 'sonarqube-scanner-621'
    }
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
/*                        dependencyCheck additionalArguments: '''
                            --scan \'./\'
                            --out \'./\'
                            --format \'ALL\'
                            --prettyPrint''',  nvdCredentialsId: 'NVD-API-Key', odcInstallation: 'OWASP-DepCheck-11'

                        //Fail if critical dependency found
                        dependencyCheckPublisher failedTotalCritical: 1, pattern: 'dependency-check-report.xml', stopBuild: true

                        //Publish HTML Report
                        publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, keepAll: false, reportDir: 'dependency-check-report.html', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
*/
                    }
                }
            }
        }
        stage ('Unit Testing') {
            steps {
/*                withCredentials([usernamePassword(credentialsId: 'mongo-db-credentials', passwordVariable: 'MONGO_PASSWORD', usernameVariable: 'MONGO_USERNAME')]) {
                    powershell 'npm test'
                }
*/          
                    echo "Unit Teting"
            }
        }
        stage ('Code Coverage') {
            steps {
                // withCredentials([usernamePassword(credentialsId: 'mongo-db-credentials', passwordVariable: 'MONGO_PASSWORD', usernameVariable: 'MONGO_USERNAME')]) {
                //     powershell 'npm run coverage'
                // }
                echo "Code Coverage"
            }
        }
        stage ('SAST - SonarQube') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    withSonarQubeEnv('sonar-qube-server') {
                        //powershell 'C:/sonar-scanner-6.2.1.4610-windows-x64/bin/sonar-scanner -D"sonar.projectKey=Solar-System-Project" -D"sonar.sources=app.js" -D"sonar.host.url=http://localhost:9000" -D"sonar.login=sqp_3adefbe51facae9c8bb00876734bf5d7f74e5b59" -D"sonar.javascript.lcov.reportPaths=./coverage/lcov.info"'
                        powershell 'C:/sonar-scanner-6.2.1.4610-windows-x64/bin/sonar-scanner -D"sonar.projectKey=Solar-System-Project" -D"sonar.sources=app.js" -D"sonar.host.url=http://localhost:9000" -D"sonar.javascript.lcov.reportPaths=./coverage/lcov.info"'
                    }
                    waitForQualityGate abortPipeline: true
                }
                    // powershell '''
                    // sonar-scanner -Dsonar.projectKey=Solar-System-Project
                    // -Dsonar.sources=app.js
                    // -Dsonar.host.url=http://localhost:9000
                    // -Dsonar.login=sqp_3adefbe51facae9c8bb00876734bf5d7f74e5b59
                    // '''
                    // powershell '''
                    // sonar-scanner \
                    //     -Dsonar.projectKey=Solar-System-Project \
                    //     -Dsonar.sources=. \
                    //     -Dsonar.host.url=http://localhost:9000 \
                    //     -Dsonar.login=sqp_3adefbe51facae9c8bb00876734bf5d7f74e5b59
                    // '''
            }
        }
    }
}