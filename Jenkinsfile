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
                archiveArtifacts artifacts: 'target/*.jar'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'

                junit(
                    testResults: 'target/surefire-reports/TEST-*.xml',
                    keepLongStdio: true
                )
            }
        }

        stage('Integration Testing') {
            steps {
                sh 'echo Running integration tests...'
                sh "echo Sleeping for ${params.SLEEP_TIMER}"
                sh "sleep ${params.SLEEP_TIMER}"

                sh "echo Running integration tests on port ${params.APPLICATION_PORT}"

                sh """
                    curl -v http://localhost:${params.APPLICATION_PORT}/hello
                """
                sh 'echo Integration testing completed'
            }
        }
    }
}
