pipeline {
    agent {
        label 'private-agent' 
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'rds_redis', url: 'https://github.com/noranzaki/jenkins_nodejs_example.git'
            }
        }
        
        stage('Build') {
            steps {
                script {
                    sh 'docker build -t nodejs-app .'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    withCredentials([
                        string(credentialsId: 'rds_hostname', variable: 'RDS_HOSTNAME'),
                        string(credentialsId: 'rds-username', variable: 'RDS_USERNAME'),
                        string(credentialsId: 'rds-password', variable: 'RDS_PASSWORD'),
                        string(credentialsId: 'rds-port', variable: 'RDS_PORT'),
                        string(credentialsId: 'redis-hostname', variable: 'REDIS_HOSTNAME'),
                        string(credentialsId: 'redis-port', variable: 'REDIS_PORT')
                    ]) {
                        sh '''
                        docker run --rm -d -p 3000:3000 \
                        -e RDS_HOSTNAME=$RDS_HOSTNAME \
                        -e RDS_USERNAME=$RDS_USERNAME \
                        -e RDS_PASSWORD=$RDS_PASSWORD \
                        -e RDS_PORT=$RDS_PORT \
                        -e REDIS_HOSTNAME=$REDIS_HOSTNAME \
                        -e REDIS_PORT=$REDIS_PORT \
                        nodejs-app
                        '''
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline ended successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}
