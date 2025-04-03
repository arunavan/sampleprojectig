pipeline {
    agent any
  	tools{
		maven 'maven1'
	}
	
    stages {
            stage('Compile and Clean') { 
                steps {
                       sh 'mvn compile'
                      }
            }
       
	        stage('Junit5 Test') { 
                 steps {
	                sh 'mvn test'
                  }
            }

	    stage('Jacoco Coverage Report') {
        	     steps{
            		//jacoco()
            		echo 'TestCoverage'
		          }
	        }
		stage('SonarQube'){
			steps{
			//	bat label: '', script: '''mvn sonar:sonar \
			//	-Dsonar.host.url=http://CDLVDIDEVMAN500:9000 \
			//	-Dsonar.login=c0909bf6713cd534393d47364d1da553431a220d'''
			echo 'Sonar Code Scanning '
				}	
   			}
        stage('Maven Build') { 
            steps {
                sh 'mvn clean install'
                  }
            }
stage('docker test') { 
	environment {
    DOCKER_HOST = "tcp://host.docker.internal:2375"
}
	    steps {
        sh 'sudo docker ps'
    }
}
        stage('Build Docker image'){
           steps {
		   environment {
    DOCKER_HOST = "tcp://host.docker.internal:2375"
}
                      //   	docker build -t nodejs-server -f Dockerfile.arg --build-arg UBUNTU_VERSION=18.04
		             //--build-arg CUDA_VERSION=10.0
                     //bat 'docker build -t  docker.repository.esi.adp.com/clientcentral/training:docker_jenkins_springboot:${BUILD_NUMBER} .'
           	    sh 'sudo docker build -t  sampleproject .'
		         }
             }
        stage('Docker Login'){
            steps {
		    
              echo "docker login from console"
                //docker login docker.repository.esi.adp.com -u clientcentralcicd -p $adpdtrrepopassword
            }                
        }
        stage('Docker Push'){
		environment {
    DOCKER_HOST = "tcp://host.docker.internal:2375"
}
            steps {
                sh 'sudo docker push aruna708/sampleproject'
            }
        }
        stage('Docker deploy'){
		environment {
    DOCKER_HOST = "tcp://host.docker.internal:2375"
}
            steps {
                sh 'sudo docker run -itd -p  8086:8086 sampleproject'
             }
        }
    
     
    }
}
