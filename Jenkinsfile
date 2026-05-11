pipeline {
    agent any

    triggers {
        pollSCM('* * * * *')
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
                        python3 -m venv venv || true
                        . venv/bin/activate
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
                    cd "${WORKSPACE}"
                    JENKINS_NODE_COOKIE=dontKillMe nohup ./venv/bin/python app.py > flask.log 2>&1 &
                '''
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
