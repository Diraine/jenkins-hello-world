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

        stage('Run Application') {
            steps {

                sh '''
                    nohup java -jar target/*.jar \
                    --server.port=${APPLICATION_PORT} \
                    > app.log 2>&1 &
                '''

                sh 'echo Waiting for application to start...'

                sh "sleep ${params.SLEEP_TIMER}"

                sh 'cat app.log'
            }
        }

        stage('Integration Testing') {
    steps {

        sh '''
            echo "Waiting for application readiness..."

            for i in {1..30}; do
                if curl -s http://localhost:${APPLICATION_PORT}/hello > /dev/null; then
                    echo "Application is ready!"
                    break
                fi

                echo "Waiting for application..."
                sleep 2
            done
        '''

        sh """
            curl -v http://localhost:${params.APPLICATION_PORT}/hello
        """

        sh 'echo Integration testing completed'
    }
}
    }

    post {
        always {
            sh 'pkill -f "java -jar" || true'
        }
    }
}
