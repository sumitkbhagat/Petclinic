pipeline {
    agent any
    
    tools{
        jdk 'jdk-11'
        maven 'maven3'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sumitkbhagat/Petclinic.git'
            }
        }
        
        stage('Code Compile') {
            steps {
               sh "mvn clean compile"
            }
        }
        
        stage('Unit Test') {
            steps {
               sh "mvn  test"
            }
        } 
        
    stage('SONAR SCANNER') {
     environment {
          sonar_token = credentials('SONAR_TOKEN')
       }
        steps {
         sh 'mvn sonar:sonar -Dsonar.projectName=$JOB_NAME \
       -Dsonar.projectKey=$JOB_NAME \
        -Dsonar.host.url=http://52.66.168.147:9000 \
        -Dsonar.token=$sonar_token'
   }
}
    }

      stage('OWASP Scan') {
       steps {
    dependencyCheck additionalArguments: '--scan ./ ', odcInstallation: 'DC'
    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
     }
}

      stage('Build Artifact') {
        steps {
       sh "mvn clean install"
     }
 }
   stage('Docker Build') {
      steps {
       script{
       withDockerRegistry(credentialsId: 'docker-cred') {
       sh "docker build -t Petclinic1 ."
      }
     }
  }
}

  stage('Docker Tag & push') {
    steps {
    script{
      withDockerRegistry(credentialsId: 'docker-cred') {
     sh "docker tag Petclinic1 sumitkumarbhagat/pet-clinic25:latest"
              }
      }
     }
  }
 }
 }


