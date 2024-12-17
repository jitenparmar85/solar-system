pipeline {
    agent any

    stages {
        stage ('Print Version') {
            steps {
                powershell 'node -v'
                powershell 'npm -v'
            }
        }
    }
}