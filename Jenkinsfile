pipeline {
    agent any

    triggers {
        pollSCM('* * * * *')  // vérifie toutes les minutes
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Taekyon/devops-158-GLN-tp'
            }
        }

        stage('Pull latest code') {
            steps {
                dir("${env.WORKSPACE}") {
                    git branch: 'main', url: 'https://github.com/Taekyon/devops-158-GLN-tp'
                }
            }
        }

        stage('Install dependencies') {
            steps {
                dir("${env.WORKSPACE}") {
                    sh '''
                        source venv/bin/activate
                        pip install flask
                    '''
                }
            }
        }

        stage('Restart Flask app') {
            steps {
                script {
                    sh 'pkill -f "python app.py" || true'
                    sh '''
                        cd "${env.WORKSPACE}"
                        source venv/bin/activate
                        nohup python app.py > flask.log 2>&1 &
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Déploiement automatique réussi ! BRAVO DAMN'
        }
        failure {
            echo 'Échec du pipeline. - AIE AIE AIE CA PUE'
        }
    }
}
