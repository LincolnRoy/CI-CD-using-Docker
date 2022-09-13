pipeline {
    agent any
	
	  tools
    {
       maven "Maven"
    }
 stages {
      stage('checkout') {
           steps {
             
               git branch: 'master', url: 'https://github.com/LincolnRoy/CI-CD-using-Docker.git'
		 // git branch: '*/master', url: 'https://github.com/LincolnRoy/CI-CD-using-Docker.git'
             
          }
        }
	 stage('Execute Maven') {
           steps {
             
                sh 'mvn package'             
          }
        }
        

  stage('Docker Build and Tag') {
           steps {
              
                sh 'docker build -t docker-repo:latest .' 
                sh 'docker tag docker-repo rootroy/docker-repo:latest'
                //sh 'docker tag docker-repo rootroy/docker-repo:$BUILD_NUMBER'
               
          }
        }
     
  stage('Publish image to Docker Hub') {
          
            steps {
        withDockerRegistry([ credentialsId: "dockerHub", url: "" ]) {
          sh  'docker push rootroy/docker-repo:latest'
        //  sh  'docker push rootroy/docker-repo:$BUILD_NUMBER' 
        }
                  
          }
        }
     
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
			{
                sh "docker run -d -p 8003:8080 rootroy/docker-repo"
 
            }
        }
 stage('Run Docker container on remote hosts') {
             
            steps {
                sh "docker -H ec2-user@172.31.88.28 run -d -p 8003:8080 rootroy/docker-repo"
 
            }
        }
    }
	}
    
