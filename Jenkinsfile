pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages{
        stage('clean') {
            steps {
                sh 'mvn clean'
            }
        }
        
       
        stage('package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('store artifact into nexus') {
            steps {
                sh 'mvn -s settings.xml clean deploy'
            }
            post {
                success {
                    echo 'artifact stored successfully'
                }
                failure {
                    echo 'failed to store'
                }
            }
        }
        stage('docker build') {
            steps {
                sh ''' 
                docker rmi -f sanjana255/nexus
                docker rm -f container
                docker build -t sanjana255/nexus .
                '''
            }
        }
        stage('docker push') {
            steps {
                sh '''
                docker login -u sanjana255 -p Biradar@24
                docker push sanjana255/nexus
                '''
            }
        }
         stage('docker pull and run') {
            steps {
                sh '''
                docker pull sanjana255/nexus
                docker run -it -d --name webapp -p 8081:8080 sanjana255/nexus
                '''
            }
        }


    }
    post {
        always {
            echo "Done"
        }
        success {
            echo "executed successfully"
        }
        failure {
            echo "execution failed"
        }
    }
}
