pipeline {
  agent any

  stages {
    parallel{
    stage('Build Artifact - Maven') {
      steps {
        sh "mvn clean package -DskipTests=true"
        archive 'target/*.jar'
      }
    }

    stage('Unit Tests - JUnit and Jacoco') {
      steps {
        sh "mvn test"
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
          jacoco execPattern: 'target/jacoco.exec'
        }
      }
    }


	stage('Mutation Tests - PIT') {
      steps {
        sh "mvn org.pitest:pitest-maven:mutationCoverage"
      }
      post {
        always {
          pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'
        }
      }
    }



 stage('SonarQube - SAST') {
      steps {
        sh "mvn clean verify sonar:sonar -Dsonar.projectKey=Jenkins -Dsonar.host.url=http://raghudevsecops.eastus.cloudapp.azure.com:9000 -Dsonar.login=sqp_40b8a947f881d4d68efa2d0cc93d5536c4222fc0"
      }
    }


    stage('Docker image build and push') {
      steps {
        sh 'docker build -t raghudev199/java-app:latest .'
        sh 'docker push raghudev199/java-app:latest'
       }
     }
   
   
   
   }
   }
 }
