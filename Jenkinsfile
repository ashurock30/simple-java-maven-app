pipeline {
    agent {
        label 'agent01'
    }
    parameters 
    {
        string(name: 'STATEMENT', defaultValue: 'hello; ls /', description: 'What should I say?')
    }
    stages {
        stage('Example') {
            steps {
                /* CORRECT */
                bat 'echo %EXAMPLE_KEY%'
            }
        }
        stage('Build') {
            steps {
                    withMaven(maven: 'Maven-3.8.6') {
                       sh 'mvn -DskipTests clean package'
                    }
            }
        }
    }
}
