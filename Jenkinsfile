pipeline {
	
    agent any
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
	    stage('docker-build'){
            steps {
                sh 'docker build -t rddevops5/webapp:2.0.0 .'
            }
            post {
                success {
                    echo 'Docker build successful'
                    
                }
            }
        }
	    stage('docker-push'){
            steps {
		    withCredentials([string(credentialsId: 'Docer-passwd', variable: 'dockerhubpsswd')]) {
			  sh "docker login -u rddevops5 -p ${dockerhubpsswd}"  // some block
                }

                sh 'docker push rddevops5/webapp:2.0.0'
            }
            post {
                success {
                    echo 'Docker push successful'
			sh "ssh ip172-18-0-21-bhgl3cj0mkfg009anhrg@direct.labs.play-with-docker.com docker run -p 8080:8080 -d --name webapp rddevops5/webapp:2.0.0"
                    
                }
            }
        }
	    
	    stage('Deploy_container_Dev'){
		    environment { 
			    //docRun  = "docker run -p 8080:8080 -d --name webapp rddevops5/webapp:2.0.0"
            }
		    
	     
            steps {
	           
		    sshagent(['dev-serv']) {
			//sh "ssh -o StrictHostKeyChecking=no root@192.168.0.53 ${docRun}"
    // some block
			}
            }
            post {
                success {
                    echo 'Docker push successful'
                    
                }
            }
        }
	      
    }
}
