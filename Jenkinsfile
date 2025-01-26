pipeline {
    agent any
    environment{
        MY_NEW_FILE='roni_file.txt'
    }

    stages {
        stage('Hello') {
            steps {
                cleanWs()
                sh 'echo "Hello World" '
                sh 'mkdir -p doron_test'
                sh 'echo Hello World > doron_test/$MY_NEW_FILE'
                sh 'cat doron_test/$MY_NEW_FILE'
            }
        }
        stage('Test'){
            steps{
                echo 'Test my code'
                sh '''
                test -f doron_test/$MY_NEW_FILE
                grep "Hello World" doron_test/$MY_NEW_FILE
                '''
            }
        }
        stage('Add docker'){
            agent{
                docker{
                image 'node:18-alpine'
                }
            }
            steps{
                sh 'npm --version'
            }
        }
    }
    post {
        success{
            archiveArtifacts artifacts: 'doron_test/**'
        }

    }
}