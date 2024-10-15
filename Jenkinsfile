pipeline {
  agent any
  tools {
        maven "3.9.9" 
   }

  stages {
      stage('clone repo') {
            steps {
              git branch: 'main', url: 'https://github.com/sai693/spring-maven.git'
            }
      }
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' 
            }  
       }
      stage('Test Maven - JUnit') {
            steps {
              sh "mvn test"
            }
            post{
              always{
                junit 'target/surefire-reports/*.xml'
              }
            }
        }
        

     }
}
