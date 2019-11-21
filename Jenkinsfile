pipeline {
	agent { label 'docker-agent' }

    environment {
        DISABLE_AUTH = 'true'
        DB_ENGINE    = 'sqlite'
    }

    stages {
        stage('build')
			agent { 
				docker { 
					label 'docker-builder'
					image 'node:8-alpine'
					args '-p 3000:3000'
				} 
			}
            steps {
                sh 'npm --version'
                sh '''
                    echo "Multiline shell steps works too"
                    ls -lah
                '''
				sh 'npm install'
            }
        }
        stage('Deploy') {
            steps {
                timeout(time: 3, unit: 'MINUTES') {
                    retry(5) {
                        sh 'echo Trying to deploy'
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'This will always run'
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