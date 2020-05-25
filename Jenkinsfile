pipeline {
    agent { docker { image 'python:3.8.3' } }
    stages {
        stage('try') {
            steps {
                
                retry(3) {
                    sh './flakey-deploy.sh'
                    sh 'echo "Hello World"'
                    sh '''
                    echo "Multiline shell steps works too"
                    ls -lah
                '''
                }

                timeout(time: 3, unit: 'MINUTES') {
                    sh './health-check.sh'
                }
            }
        }
        stage('build') {
            steps {
                
                retry(3) {
                    sh './flakey-deploy.sh'
                    sh 'python --version'
                    
                }

                timeout(time: 3, unit: 'MINUTES') {
                    sh './health-check.sh'
                }
            }
        }
    }
}
