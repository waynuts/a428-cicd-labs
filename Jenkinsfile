pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
            args '-p 3000:3000'
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
                    input message: 'Lanjutkan ke tahap Deploy? (Klik "Proceed" untuk melanjutkan atau "Abort" untuk menghentikan pipeline)'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh './jenkins/scripts/deliver.sh'
                    echo 'Menjeda selama 1 menit agar aplikasi berjalan...'
                    sleep 60
                    echo 'Mengakhiri aplikasi setelah 1 menit.'
                    sh './jenkins/scripts/kill.sh'
                }
            }
        }
    }
}
