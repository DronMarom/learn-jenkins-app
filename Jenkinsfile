pipeline {
    agent any
    environment{
        MY_NEW_FILE='roni_file.txt'
    }
    parameters {
        string(name: 'MY_NEW_FILE', defaultValue: 'roni_file.txt', description: 'Name of the file to create')
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
                image 'node:23.6.1-alpine3.20'
                reuseNode true
                }
            }
            steps{
                sh '''
                npm install
                npm ci
                npm run build
                grep learn-jenkins-app/build/index.html
                '''

            }
        }
    }
    post {
        success{
            archiveArtifacts artifacts: 'doron_test/**'
        }

    }
}