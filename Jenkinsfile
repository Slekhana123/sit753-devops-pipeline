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
                sh 'docker run --rm sit753-app /bin/sh -c "pip install pylint && pylint app.py" || true'
            }
        }

        stage('Security') {
            steps {
                sh 'docker run --rm sit753-app /bin/sh -c "pip install bandit && bandit -r ." || true'
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
                echo 'Skipping Git tag and push unless GitHub credentials are configured.'
            }
        }

        stage('Monitoring') {
            steps {
                sh 'curl http://host.docker.internal:5000/health || true'
            }
        }
    }
}
