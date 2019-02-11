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
		stage('Docker'){
          sh '''
            docker build -t rddevops5/webapp:2.0.0 .
          '''
       }
        
    }
}
