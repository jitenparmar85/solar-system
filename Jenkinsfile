pipeline {
    agent any

    stages {
        stage ('Install Dependancies') {
            steps {
                echo "Install Dependencies from package.json"
                powershell 'npm install --no-audit'
            }
        }
    }
}