pipeline {
    agent any
    tools{
        git 'Default'
    }
    stages {
        stage ('GetProject') {
            steps {
                git branch:'master', url:'https://github.com/CraigQ-College/demo.git'
            }
        }
        stage ('build') {
            steps {
                sh 'mvn clean:clean'
                sh 'mvn dependency:copy-dependencies'
                sh 'mvn compiler:compile'
            }
        }
        stage ('Package') {
            steps {
                sh 'mvn package'
            }
        }
	stage ('Archive') {
            steps {
                archiveArtifacts allowEmptyArchive: true,
             	artifacts:'**/demo*.war'
            }
        }
	stage ('Deploy') {
            steps {
                sh 'docker build -f Dockerfile -t myapp . ' 
                sh 'docker rm -f "myappcontainer" || true'
                sh 'docker run --name "myappcontainer" -p 8081:8080 --detach myapp:latest'
            }
        }
    }
}
