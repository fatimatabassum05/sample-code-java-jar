pipeline{
    agent none
    stages{
       stage('Git Checkout Stage'){
            agent {label 'sonar_node'}
            steps{
                git branch: 'main', url: 'https://github.com/fatimatabassum05/sonarqube-example.git'
            }
         }        
       stage('Build Stage'){
            agent {label 'sonar_node'}
            steps{
                sh 'mvn clean install'
            }
         }
        stage('SonarQube Analysis Stage') {
            agent {label 'sonar_node'}
            steps{
                withSonarQubeEnv('sonarqube_1') { 
                    sh "mvn clean verify sonar:sonar -Dsonar.projectKey=sonarqube_1"
                }
            }
        }
        stage('Build docker Image'){
          agent {label 'docker_node_new'}
          steps{
            sh 'docker build -t fatimatabassum/fatima12:${BUILD_NUMBER} .'
          }
        }
        stage('Push To Dockerhub'){
          agent {label 'docker_node_new'}
          steps{
            sh 'docker push fatimatabassum/fatima12:${BUILD_NUMBER}'
          }
        }
        stage('Deploy Stage') {
          agent {label 'docker_node_new'}
          steps{
            sh 'docker run -d -p 80:8080 fatimatabassum/fatima12:${BUILD_NUMBER}'
          } 
        }
    }
}
