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
                sh 'pytest || true'
            }
        }
        stage('Code Quality') {
            steps {
                sh 'pip install pylint && pylint app.py || true'
            }
        }
        stage('Security') {
            steps {
                sh 'pip install bandit && bandit -r . || true'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker run -d -p 5000:5000 --name flaskapp sit753-app'
            }
        }
        stage('Release') {
            steps {
                sh 'git tag v1.0.${BUILD_NUMBER}'
                sh 'git push origin v1.0.${BUILD_NUMBER}'
            }
        }
        stage('Monitoring') {
            steps {
                sh 'curl http://localhost:5000/health || true'
            }
        }
    }
}
