pipeline {
    agent any
    stages{
        stage('git cloned'){
            steps{
                git url:'https://github.com/manjugdr/php-project-CI-CD/', branch: "master"
              
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t manjugdr/2febimg:v2 .'
                    sh 'docker images'
                }
            }
        }
        stage('Static Code Analysis') {
        environment {
        SONAR_URL = "http://65.0.204.6:9000"
      }
      steps {
        withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_AUTH_TOKEN')]) {
          sh 'cd/var/lib/jenkins/workspace/php && mvn sonar:sonar -Dsonar.login=$SONAR_AUTH_TOKEN -Dsonar.host.url=${SONAR_URL}'
        }
      }
    }
          stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-cred', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push manjugdr/2febimg:v2'
                }
            }
        }
        
     stage('Deploy') {
            steps {
               script {
                   def dockerrm = 'sudo docker rm -f My-first-containe221 || true'
                    def dockerCmd = 'sudo docker run -itd --name My-first-containe221 -p 8082:80 manjugdr/2febimg:v2'
                    sshagent(['sshkeypair']) {
                        //chnage the private ip in below code
                        // sh "docker run -itd --name My-first-containe211 -p 8082:80 manjugdr/2febimg:v1"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.44.169 ${dockerrm}"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.44.169 ${dockerCmd}"
                    }
                }
            }
        }
    }
}
