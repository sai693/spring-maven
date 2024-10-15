pipeline {
  agent any
  tools {
    maven "Maven 3.8.6"
  }

  stages {
    stage('Clone repo') {
      steps {
        git branch: 'main', url: 'https://github.com/sai693/spring-maven.git'
      }
    }

    stage('Build Artifact') {
      steps {
        sh "mvn clean package -DskipTests=true"
        archiveArtifacts artifacts: 'target/*.jar', allowEmptyArchive: true
      }
    }

    stage('Test Maven - JUnit') {
      steps {
        sh "mvn test"
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
        }
      }
    }

    stage('Docker Build') {
      steps {
        script {
          // Build Docker image
          sh 'docker build -t spring-maven-app:latest .'
        }
      }
    }

    stage('Run Docker Container') {
      steps {
        script {
          // Run Docker container, mapping host port 8086 to container port 8080
          sh 'docker run -d -p 8086:8080 --name spring-maven-container spring-maven-app:latest'
        }
      }
    }
  }

  post {
    always {
      // Clean up Docker container after pipeline execution
      script {
        sh 'docker stop spring-maven-container || true'
        sh 'docker rm spring-maven-container || true'
      }
    }
  }
}
