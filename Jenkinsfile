pipeline {
    agent any

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'pytest test_app.py'
            }
        }
        stage('Deploy') {
            when {
                any0f {
                    branch 'main'
                    branch pattern: "relase/.*", comparator: "REGEXP"
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
                    content: "✅ 🙆🏻 Build SUCCESS on `${env.BRANCH_NAME}`\nURL: ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'https://discordapp.com/api/webhooks/1369608821504348230/SoXebbLb_RhnG5Yhrzz13N3kKJOfmM3-XTQcvdXgVSFN2jNl9kB5UM0z79IAxbOplhDP'
                )
            }
        }
        failure {
            script {
                def payload = [
                    content: "❌ 🙅🏻‍♀️ BUild FAILED on `${env.BRANCH_NAME}`\nURL: ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'https://discordapp.com/api/webhooks/1369608821504348230/SoXebbLb_RhnG5Yhrzz13N3kKJOfmM3-XTQcvdXgVSFN2jNl9kB5UM0z79IAxbOplhDP'
                )
            }
        }
    }
}