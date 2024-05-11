pipeline {
    agent any
    tools {
        nodejs 'nodejs-22-1-0'
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh || echo "No such process"'
 
            }
        }
        stage('Upload artifact'){
            steps {
                sh 'zip -r webapp.zip build/*'
                archiveArtifacts artifacts: 'webapp.zip', followSymlinks: false
            }
        }
        stage('Config deploy'){
            steps {
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
            }
        }
        stage('Deploy to Stage'){
            steps {
                sh 'cp -r build/* /var/www/html'
            }
        }
    }
}
