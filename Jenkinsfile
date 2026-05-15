pipeline {
    agent any

    tools {
        // Install the Maven version configured as "maven-398" and add it to the path.
        maven "maven-398"
    }

    stages {
        stage('Check Maven') {
            steps {
                sh "if ! command -v mvn; then echo 'Maven not found!'; exit 1; fi"
            }
        }

        stage('Build') {
            steps {
                git branch: 'main', url: 'http://localhost:5555/dasher-org/jenkins-hello-world.git'
                sh "mvn clean package -DskipTests=true"
            }
        }

        stage('Unit Test') {
            steps {
                sh "mvn test"
            }
        }
    }
}
pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'mvn clean package -DskipTests=true'
        archiveArtifacts 'target/hello-demo-*.jar'
      }
    }

    stage('Test') {
      steps {
        sh 'mvn test'
        junit(testResults: 'target/surefire-reports/TEST-*.xml', keepProperties: true, keepTestNames: true)
      }
    }
    
    stage('Integration Testing') {
      steps {
        sh "sleep 10s"
        sh 'echo Testing using cURL commands......'
      }
    }
  }
  tools {
    maven 'M398'
  }

}
