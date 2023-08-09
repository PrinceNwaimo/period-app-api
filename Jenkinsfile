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


// pipeline {
//     agent any

//     environment {
//         AWS_ACCESS_KEY_ID = credentials('AKIARE4ABVEADEWU6K5S')
//         AWS_SECRET_ACCESS_KEY = credentials('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')
//         AWS_DEFAULT_REGION = 'us-east-1'
//         EC2_INSTANCE_IP = '44.204.157.79'
//         SSH_KEY = credentials('my-key')
//     }

//     stages {
//         stage('Checkout') {
//             steps {
//                 checkout scm
//             }
//         }

//         stage('Build') {
//             steps {
//                 sh './mvnw clean package'
//             }
//         }

//         stage('Deploy') {
//             steps {
//                 script {

//                     withCredentials([sshUserPrivateKey(credentialsId: my-key, keyFileVariable: 'my-key')]) {
//                         sh "scp -i ${my-key} target/your-app.jar ec2-user@${44.204.157.79}:~/"
//                     }

//                     withCredentials([sshUserPrivateKey(credentialsId: my-key, keyFileVariable: 'my-key')]) {
//                         sh "ssh -i ${my-key} ec2-user@${44.204.157.79} 'nohup java -jar ~/your-app.jar > app.log 2>&1 &'"
//                     }
//                 }
//             }
//         }
//     }
// }


pipeline {
                agent any

                environment {
                    AWS_ACCESS_KEY_ID = credentials('AKIARE4ABVEADEWU6K5S')
                    AWS_SECRET_ACCESS_KEY = credentials('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')
                    AWS_DEFAULT_REGION = 'us-east-1'
                    DOCKER_REGISTRY_URL = 'docker.io/kingaby/periodapp'
                    DOCKER_REGISTRY_CREDENTIALS = credentials('Docker')
                    EC2_INSTANCE_IP = '44.204.157.79'
                    SPRING_PROFILE = 'prod'
                }

                stages {
                    stage('Checkout') {
                        steps {
                            checkout scm
                        }
                    }

                    stage('Build and Push Docker Image') {
                        steps {
                            script {
                                def imageName = ":${env.BUILD_NUMBER}"

                                // Build Docker image
                                sh "docker build -t ${imageName} ."

                                
                                withCredentials([string(credentialsId: "${Docker}", variable: 'DOCKER_REGISTRY_CREDS')]) {
                                    sh "docker login -u <username> -p ${DOCKER_REGISTRY_CREDS} ${docker.io/kingaby/periodapp}"
                                }

                    
                                sh "docker push ${imageName}"
                            }
                        }
                    }

                    stage('Deploy to EC2') {
                        steps {
                            script {
                                
                                withCredentials([string(credentialsId: 'your-ssh-key-id', variable: 'SSH_KEY_FILE')]) {
                                    sh "ssh -i ${SSH_KEY_FILE} ec2-user@${44.204.157.79} 'docker stop your-app-container-name || true && docker rm your-app-container-name || true'"
                                    sh "ssh -i ${SSH_KEY_FILE} ec2-user@${44.204.157.79} 'docker run -d --name your-app-container-name -p 8080:8080 -e SPRING_PROFILES_ACTIVE=${SPRING_PROFILE} ${DOCKER_REGISTRY_URL}/${imageName}'"
                                }
                            }
                        }
                    }
                }
            }

        }
    }
}


