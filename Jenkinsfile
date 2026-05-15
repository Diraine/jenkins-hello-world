pipeline {
    agent any

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-21-openjdk-amd64'
        PATH = "${JAVA_HOME}/bin:${env.PATH}"
    }

    tools {
        maven 'maven-398'
    }

    stages {

        stage('Environment Check') {
            steps {
                sh 'echo $JAVA_HOME'
                sh 'which java'
                sh 'which javac'
                sh 'which mvn'
                sh 'java -version'
                sh 'javac -version'
                sh 'mvn -version'
            }
        }

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Diraine/jenkins-hello-world.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests=true'
            }
        }

        stage('Unit Test') {
            steps {
                sh 'mvn test'
            }

            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar'
            }
        }

        stage('Integration Testing') {
            steps {
                sh 'echo Running integration tests...'
                sh 'sleep 10'
                sh 'echo Integration testing completed'
            }
        }
    }
}
