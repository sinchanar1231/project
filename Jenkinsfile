pipeline {
    agent any

    tools{
        maven 'maven'
    }

    stages{
        stage('Check and remove container'){
            steps{
                script{
                    def containerExists = sh(script: "docker ps -q -f name=sinchu", returnStdout: true).trim()
                    if (containerExists) {
                    sh "docker stop sinchu"//container name
                    sh "docker rm sinchu"//container name
                    }
                }
            }
        }
        stage('Build package'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('Create image'){
            steps{
                sh 'sudo docker build -t app /var/lib/jenkins/workspace/project/' //jobname
            }
        }
        stage('Assign tag'){
            steps{
                sh 'docker tag app sinchana1231/apachetomcat'
            }
        }
        stage('Push to dockerhub'){
            steps{
                sh 'echo "Mydocker@12" | docker login -u "sinchana1231" --password-stdin'
                sh 'docker push sinchana1231/apachetomcat'
            }
        }
        stage('Remove images'){
            steps{
                sh 'docker rmi -f $(docker images -q)'
            }
        }
        stage('Pull image from DockerHub'){
            steps{
                sh 'docker pull sinchana1231/apachetomcat'
            }
        }
        stage('Run a container'){
            steps{
                sh 'docker run -it -d --name sinchu -p 8081:8080 sinchana1231/apachetomcat'
            }
        }
    }
    post {
        success {
            echo 'Deployment successful'
        }
        failure {
            sh 'docker rm -f sinchu'
        }
        always{
            echo 'Deployed'
        }
    }

}



