pipeline {
    agent { docker { image 'python:3.8.3' } }
    stages {
        stage('try') {
            steps {
               timeout(time: 3, unit: 'MINUTES') {
                    retry(5) {
                        //sh './flakey-deploy.sh'
                        // sth that will fail
                    }
                }
                sh 'echo "Hello World"'
                sh '''
                    echo "Multiline shell steps works too"
                    ls -lah
                '''
            }
        }
        stage('build') {
            steps {
                    sh 'python --version'
                    sh './gradlew build'
                    
                }
            }
        stage('Test') {
            steps {
                sh './gradlew check'
                sh 'echo "Fail!";'//exit 1

            }
        }
        stage('sqlte'){

            agent {
                label '!windows'
             }

            environment {
                 DISABLE_AUTH = 'true'
                 DB_ENGINE    = 'sqlite'
             }

            steps {
                echo "Database engine is ${DB_ENGINE}"
                echo "DISABLE_AUTH is ${DISABLE_AUTH}"
                sh 'printenv'
            }
        }
    }
    post {
        always {
            echo 'This will always run'
            archiveArtifacts artifacts: 'build/libs/**/*.jar', fingerprint: true
            junit 'build/reports/**/*.xml'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}
