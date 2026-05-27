pipeline {
    agent any

    tools {
        // This must match the Name of the Maven installation configured in Jenkins 
        // under Global Tool Configuration (Manage Jenkins -> Tools)
        maven 'Maven 3.9' 
    }

    environment {
        // You can define variables here to use throughout the script
        APP_NAME = 'kaiwara-site'
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Pulls the latest code from the Git repository configured in the Jenkins job
                checkout scm
            }
        }

        stage('Validate & Clean') {
            steps {
                echo 'Cleaning up previous builds...'
                // sh is used for Linux/Mac agents. For Windows, use bat 'mvn clean'
                sh 'mvn clean'
            }
        }

        stage('Compile Code') {
            steps {
                echo 'Compiling Java source files...'
                sh 'mvn compile'
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Running JUnit tests...'
                sh 'mvn test'
            }
        }

        stage('Package Application') {
            steps {
                echo "Packaging application into a WAR/JAR file..."
                sh 'mvn package -DskipTests' 
            }
        }
    }

    post {
        always {
            echo 'Archiving build artifacts...'
            // This saves your compiled .war or .jar file inside Jenkins so you can download it later
            archiveArtifacts artifacts: 'target/*.war', fingerprint: true, allowEmptyArchive: true
        }
        success {
            echo 'Pipeline completed successfully! Ready for deployment.'
        }
        failure {
            echo 'Pipeline failed. Please check the logs above to see which Maven stage failed.'
        }
    }
}
