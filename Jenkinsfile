pipeline {
    agent any 
    stages {
        stage('Build') {
            steps {
                    withMaven(maven: 'Maven-3.8.6') {
                       sh 'mvn -DskipTests clean package'
                    }
            }
        }
    }
}
