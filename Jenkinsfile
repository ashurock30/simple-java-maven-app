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

        stage('Clean Maven Local Repo') {
            steps {
                sh 'echo "Deleting Maven local repo"'
                sh 'rm -rf /opt/maven/.repository/*'
            }
        }

        stage('Build') {
            steps { 
                withMaven(globalMavenSettingsConfig: '', jdk: 'JDK11', maven: 'Maven-3.9.5', mavenSettingsConfig: 'maven-settings', traceability: true) {
                    sh 'mvn clean install deploy'
                    //sh 'mvn clean install'
                }
            }
        }

        stage("Test Application") {
           steps {
               withMaven(globalMavenSettingsConfig: '', jdk: 'JDK11', maven: 'Maven-3.9.5', mavenSettingsConfig: 'maven-settings', traceability: true) { 
                   sh "mvn test"
               }
           }
       }

       stage('Sonar Qube Scan With Project Creation') {
           steps {
               withSonarQubeEnv(credentialsId:'sonar-test',installationName:'SonarQube') {
                   withMaven(globalMavenSettingsConfig: '', jdk: 'JDK11', maven: 'Maven-3.9.5', mavenSettingsConfig: 'maven-settings', traceability: true) {
                       sh 'mvn sonar:sonar -Dsonar.projectKey=com.mycompany.app:my-app -Dsonar.projectName="Sonar-MyApp"'
                   } 
               }

           }
       }

       stage("Quality Gate"){
           steps {
               script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-test'
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
