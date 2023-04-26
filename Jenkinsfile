pipeline {
    environment {
        registry = "mahach/projet"
        registryCredential = 'dockerhub_id'
        dockerImage = 'projet'
    }
    agent any
    stages {
        stage('Git') {
            steps {
                echo "Getting Project from Git";
                sh "rm -rf Project"
                sh "git clone https://github.com/MahaChouchene/Project.git"
                  }
            }
            
        stage('MVN Clean'){  
            steps {
                sh "mvn clean"
                  }
        }        
        
        stage('MVN Install'){
            steps {
                sh "mvn install"
                  }
        }       
        
        stage('MVN Test'){
            steps {
                sh "mvn test"
                  }
        }          
        
        stage('MVN Package'){
            steps {
                sh "mvn package"
                  }
        }          
        
        stage('MVN SONARQUBE'){
            steps{
               mvn "sonar:sonar \
  -Dsonar.projectKey=mahaexam \
  -Dsonar.host.url=http://192.168.1.125:9000 \
  -Dsonar.login=9586c59b328770626e70afd27de89bd80bfc811d"
                 }
        }    
        stage('MVN NEXUS'){
            steps {
                sh 'mvn deploy -Dmaven.test.skip=true'
                  }
        }
        stage('Building our image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Deploy our image') {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Cleaning up') {
            steps {
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }
   }
}
