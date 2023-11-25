pipeline {
    agent {
        label 'DEVLINUX'
    }

    environment {
        // Define the Maven installation configured in Jenkins
        MAVEN = tool 'Maven-3.9.5'
        JAVA = tool 'JDK11'
    }

    stages{

        stage('Checkout external proj') {
            steps {
                git branch: 'master',
                    //credentialsId: 'gitlab-cred',
                    url: 'https://github.com/ashurock30/simple-java-maven-app.git'
            }
        }

        stage('Build') {
            steps { 
                withMaven(globalMavenSettingsConfig: '', jdk: 'JDK11', maven: 'Maven-3.9.5', mavenSettingsConfig: 'maven-settings', traceability: true) {
                    sh 'mvn clean install'
                }
            }
        }
    }

    post {
        always {
            cleanWs()
            echo "Workspace Cleaned"
        }
        success {
            // Actions to be taken if the build is successful
            echo 'Build successful!'

            // Example: Trigger downstream jobs or deployments
            // build job: 'Deploy-App', wait: false
        }
        failure {
            // Actions to be taken if the build fails
            echo 'Build failed!'
        }
    }
}