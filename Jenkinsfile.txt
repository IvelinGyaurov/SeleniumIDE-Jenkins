
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Restore') {
            steps {
                bat 'dotnet restore SeleniumIDE.sln'
            }
        }

        stage('Build') {
            steps {
                bat 'dotnet build SeleniumIDE.sln --configuration Release --no-restore'
            }
        }

        stage('Test') {
            steps {
                bat 'dotnet test SeleniumIDE.sln --configuration Release --no-build --logger "console;verbosity=normal"'
            }
        }
    }

    post {
        success {
            echo 'Build and tests completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the Console Output.'
        }
    }
}
