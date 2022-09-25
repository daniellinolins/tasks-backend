pipeline {
    agent any
    stages {
        stage ('Build Backend') {
            steps {
                bat 'mvn clean package -DskipTest=true'
            }
        }
        stage ('Backend Unit Test') {
            steps {
                bat 'mvn test'
            }
        }        
    }

}