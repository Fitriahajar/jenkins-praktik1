pipeline {
    agent {
         docker {
             image 'python:3.10'
         }
    }

    environment {
        VENV = 'venv'
    } 
 
    stages {
        stage('Setup Environment & Install Dependencies') {
            steps {
                sh '''
                    python -m venv $VENV
                    . $VENV/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                 '''
            }
        }
        stage('Run Tests') {
            steps {
                sh '''
                    . $VENV/bin/activate
                    pytest test_app.py
                '''
            }
        }
        stage('Deploy') {
            when {
                anyOf {
                    branch 'main'
                    branch pattern: "release/.*", comporator: "REGEXP"
                }
            }
            steps {
                echo "Simulating deploy from branch ${env.BRANCH_NAME}"
            }
        }
    }

    post {
        success {
            script {
                def payload = [
                    content: "Build SUCCESS on '${env.BRANCH_NAME}'\nURL: ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.jsonOutput.tojson(payloaod),
                    url: 'https://discord.com/api/webhooks/ISI_WEBHOOK_DI_SINI'
                )
            }
        }
        failure {
            script {
                def payload = [
                    content: "Build FAILED on '${env.BRANCH_NAME}'\nURL: ${env.BUILD URL}"
                ]
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.jsonOutput.toJson(payload),
                    url: 'https://discord.com/api/webhook/ISI_WEBHOOK_DI_SINI'
                )
            }
        }
    }
}
