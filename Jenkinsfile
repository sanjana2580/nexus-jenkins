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
        stage('validate') {
            steps {
                sh 'mvn validate'
            }
        }
        stage('compile') {
            steps {
                sh 'mvn compile'
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