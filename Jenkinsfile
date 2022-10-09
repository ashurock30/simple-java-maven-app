pipeline {
    agent any 
    stages {
        stage('Build') {
            steps {
                step {
                    withMaven(maven: 'Maven-3.8.6') {
                        'mvn -DskipTests clean package'
                    }
                }
            }
        }
    }
}
