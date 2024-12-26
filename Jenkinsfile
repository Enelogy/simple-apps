pipeline {
    agent { label 'server1-irfan' }

    stages {
        stage('Pull SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/Enelogy/simple-apps.git'
            }
        }
        
        stage('Build') {
            steps {
                sh'''
                cd apps
                npm install
                '''
            }
        }
        
        stage('Testing') {
            steps {
                sh'''
                cd apps
                npm test
                npm run test:coverage
                '''
            }
        }
        
        stage('Code Review') {
            steps {
                sh'''
               sonar-scanner \
                -Dsonar.projectKey=simple-apps \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://172.23.14.31:9000 \
                -Dsonar.login=sqp_5762a5b1cc0380eb62aa5efec2fb004a4b6c2a97
                '''
            }
        }
        
        stage('Change file env') {
            steps {
                sh'''
                cd apps
                sed -i 's/localhost/db/g' .env
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh'''
                docker compose up --build -d
                '''
            }
        }
        
        stage('Backup') {
            steps {
                 sh 'docker compose push' 
            }
        }
    }
}