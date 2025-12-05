pipeline{
    agent any

    stages {
        stage('Checkout code') {
            steps {
                git branch: 'main', url: 'https://github.com/Ayrola/SeleniumWebDriverTests_Workshop_1_Task_2.git'
            }
        }

        stage('Setup .net') {
            steps {
                bat '''
                echo Setting up .NET 8.0 SDK
                choco install dotnet-8.0-sdk -y
                '''
            }
        }

        stage('Restore dependencies') {
            steps {
                bat '''
                echo Restoring .NET dependencies
                dotnet restore
                '''
            }
        }

        stage('Build project') {
            steps {
                bat '''
                echo Building the project
                dotnet build
                '''
            }
        }

        stage('Run TestProject1') {
            steps {
                bat '''
                echo Running tests
                dotnet test TestProject1/TestProject1.csproj --logger "trx;LogFileName=test_results.trx"
                '''
            }
        }

        stage('Run TestProject2') {
            steps {
                bat '''
                echo Running tests
                dotnet test TestProject2/TestProject2.csproj --logger "trx;LogFileName=test_results.trx"
                '''
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
            step([
				$class: 'MSTestPublisher',
				testResultsFile: '**/TestResults/*.trx'
			])
        }
    }
}
