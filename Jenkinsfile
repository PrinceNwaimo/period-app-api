// pipeline {
//   agent any

//   stages {
//     stage('Build') {
//       steps {
//         sh 'mvn clean package -Dprod'
//       }
//     }

//     stage('Build Docker Image') {
//       steps {
//         script {
//           docker.withRegistry('https://hub.docker.com/', 'docker-hub-id') {
//             def dockerImage = docker.build('period-app-api:latest', '.')
//             dockerImage.push()
//           }
//         }
//       }
//     }

//     stage('Deploy to EC2') {
//       steps {
//         script {
//           sh 'ssh -i ~/Downloads/raheem-jenkins-one-note-keypair.pem ec2-user@ec2-52-90-67-9.compute-1.amazonaws.com "docker-compose down && docker-compose up -d"'
//         }
//       }
//     }
//   }
// }


pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = credentials('AKIARE4ABVEADEWU6K5S')
        AWS_SECRET_ACCESS_KEY = credentials('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')
        AWS_DEFAULT_REGION = 'us-east-1'
        EC2_INSTANCE_IP = '54.164.38.12354.164.38.123'
        SSH_KEY = credentials('my-key')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh './mvnw clean package'
            }
        }

        stage('Deploy') {
            steps {
                script {

                    withCredentials([sshUserPrivateKey(credentialsId: my-key, keyFileVariable: 'my-key')]) {
                        sh "scp -i ${my-key} target/your-app.jar ec2-user@${54.164.38.12354.164.38.123}:~/"
                    }

                    withCredentials([sshUserPrivateKey(credentialsId: my-key, keyFileVariable: 'my-key')]) {
                        sh "ssh -i ${my-key} ec2-user@${54.164.38.12354.164.38.123} 'nohup java -jar ~/your-app.jar > app.log 2>&1 &'"
                    }
                }
            }
        }
    }
}



