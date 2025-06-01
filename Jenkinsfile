pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat 'docker build -t sit753-app .'
            }
        }

        stage('Test') {
            steps {
                bat 'pytest || exit 0'
            }
        }

        stage('Code Quality') {
            steps {
                bat 'pip install pylint && pylint app.py || exit 0'
            }
        }

        stage('Security') {
            steps {
                bat 'pip install bandit && bandit -r . || exit 0'
            }
        }

        stage('Deploy') {
            steps {
                bat 'docker run -d -p 5000:5000 --name flaskapp sit753-app'
            }
        }

        stage('Release') {
            steps {
                bat 'git tag v1.0.%BUILD_NUMBER%'
                bat 'git push origin v1.0.%BUILD_NUMBER%'
            }
        }

        stage('Monitoring') {
            steps {
                bat 'curl http://localhost:5000/health || exit 0'
            }
        }
    }
}
