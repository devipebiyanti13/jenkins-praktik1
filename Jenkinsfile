pipeline {
    agent any

    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    // Create and activate virtual environment
                    sh 'python3 -m venv venv'
                    sh 'source venv/bin/activate'
                    // Install dependencies
                    sh 'pip install -r requirements.txt'
                }
            }
        }
        stage('Run Tests') {
            steps {
                script {
                    // Ensure virtual environment is activated before running tests
                    sh 'source venv/bin/activate && pytest test_app.py'
                }
            }
        }
        stage('Deploy') {
            when {
                anyOf {
                    branch 'main'
                    branch pattern: "release/.*", comparator: "REGEXP"
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
                    content: "❌ 🙅🏻‍♀️ Build FAILED on `${env.BRANCH_NAME}`\nURL: ${env.BUILD_URL}"
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
