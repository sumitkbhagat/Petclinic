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
    }
}
        
   
