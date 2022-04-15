def secret = 'Server'
def server = 'app-sigit1@103.172.205.236'
def directory = 'Frontend-Wayshub'
def branch = 'main'

pipeline{
    agent any
    stages{
        stage ('docker delete & git pull'){
            steps{
                sshagent([secret]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}
                    docker-compose down
                    docker system prune -f
                    git pull origin ${branch}
                    exit
                    EOF"""
                }
            }
        }
        stage ('docker compose'){
            steps{
                sshagent([secret]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}
                    docker build -t sigit26/ways-fe:2.0 .
                    exit
                    EOF"""
                }
            }
        }
        stage ('docker up'){
            steps{
                sshagent([secret]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}
                    docker-compose up -d
                    exit
                    EOF"""
                }
            }
        }
    }
}
