pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
            args '-p 6000:6000'
        }
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
        stage('Manual Approval') { 
            steps {
                script {
                    def userInput = input message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed', parameters: [choice(choices: 'Proceed\nAbort', description: 'Pilih tindakan:', name: 'ACTION')]
                    if (userInput == 'Abort') {
                        error("Pipeline dihentikan oleh pengguna.")
                    }
                }
            }
        }
        stage('Deploy') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
                 sleep(time: 60, unit: 'SECONDS')
                sh './jenkins/scripts/kill.sh' 
            }
        }
    }
}