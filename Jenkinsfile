pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'docker build -t sit753-app .'
            }
        }

        stage('Test') {
            steps {
                sh 'docker run --rm sit753-app pytest || true'
            }
        }

        stage('Code Quality') {
            steps {
                sh 'docker run --rm sit753-app pylint app.py || true'
            }
        }

        stage('Security') {
            steps {
                sh 'docker run --rm sit753-app bandit -r . || true'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker rm -f flaskapp || true'
                sh 'docker run -d -p 5000:5000 --name flaskapp sit753-app'
            }
        }

        stage('Release') {
            steps {
                // COMMENT THIS OUT unless you want to set up GitHub credentials
                // sh 'git tag v1.0.${BUILD_NUMBER}'
                // sh 'git push origin v1.0.${BUILD_NUMBER}'
            }
        }

        stage('Monitoring') {
            steps {
                sh 'curl http://localhost:5000/health || true'
            }
        }
    }
}
