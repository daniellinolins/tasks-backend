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
        stage ('Sonar Analysis') {
            environment {
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps {
                withSonarQubeEnv('SONAR_LOCAL'){
                    bat "$scannerHome\\bin\\sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=aa43ae7c8e00039c1e99d90a44a32a477eb60d20 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**Application.java"
                }
            }
        }        
        stage ('Quality Gate') {
            steps {
                sleep(5)
                timeout(time:1, unit:'MINUTES') {
                    waitForQualityGate abortPipeline:true
                }
            }
        }        
        stage ('Deploy Backend') {
            steps {
                deploy adapters: [tomcat8(credentialsId: 'tomcatLogin', path: '', url: 'http://localhost:8080/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        }        
    }
}