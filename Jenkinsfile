pipeline {
	def dockerRun = 'docker run -p 8080:8080 -d --name webapp rddevops5/webapp:2.0.0'
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
                    
                }
            }
        }
	    
	    stage('Deploy_container_Dev'){
	     
            steps {
		   
                sshagent(['dev-serv']) {
			sh "ssh -o StrictHostKeyChecking=no root@192.168.56.101 ${dockerRun}"
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
