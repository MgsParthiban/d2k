pipeline {
    agent {
        docker {
            image 'parthitk/task:sonar4'  // Use your Docker image
        }
    }
    environment {
        DOTNET_CLI_HOME = '/tmp'
        PATH = "/usr/local/bin:${env.PATH}"   // Ensure SonarScanner is in PATH
        SONAR_URL = "http://3.109.216.229:9000/"  // SonarQube server URL
    }
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm  // Get the code from the repository
            }
        }
        stage('Restore Dependencies') {
            steps {
                sh 'dotnet restore'  // Restore NuGet dependencies
            }
        }
        stage('Build and Analyze') {
            steps {
                withCredentials([string(credentialsId: 'sonar', variable: 'SONAR_AUTH_TOKEN')]) {
                    // Step 1: Begin SonarQube analysis
                   
                    sh 'ls -l /usr/local/bin'
                    
                    //sh 'dotnet tool list -g'
                    //sh 'dotnet-sonarscanner --version'
                    sh 'dotnet-sonarscanner /?' 
                    //sh 'dotnet sonarscanner begin /k:"myapp" /d:sonar.host.url=$SONAR_URL /d:sonar.login=$SONAR_AUTH_TOKEN'
                    sh 'dotnet sonarscanner begin /k:"myapp" /d:sonar.host.url=$SONAR_URL  /d:sonar.login=$SONAR_AUTH_TOKEN'
                    //sh 'dotnet sonarscanner begin /k:"myapp" /d:sonar.host.url=$SONAR_URL  /d:sonar.login=$SONAR_AUTH_TOKEN'

                    // Step 2: Build the .NET project
                    sh 'dotnet build'

                    // Step 3: End SonarQube analysis and submit results
                    sh 'dotnet sonarscanner end /d:sonar.login=$SONAR_AUTH_TOKEN'
                }
            }
        }
    }
}
