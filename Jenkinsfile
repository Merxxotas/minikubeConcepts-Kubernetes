def remote=[:]
remote.name = 'brayanmarin'
remote.host = '172.28.92.49'
remote.allowAnyHosts = true
pipeline{
    environment{
        dockerImageName = "merxxaz/python-app:v1.0.0"
        dockerImage = ""
        SSH_CREDS = credentials('23f6de3f-0ddf-4ca8-9c0d-18960773cd0e')
    }
    agent any
    stages{
        stage('SSH Connection'){
            steps{
                script{
                    remote.user = env.SSH_CREDS_USR
                    remote.password = env.SSH_CREDS_PSW
                }
            }
        }
        stage('Checkout Source') {
            steps {
              sh 'ls'
            }
        }
        stage('Build image') {
            steps {
                script {
                    // Using Docker Pipeline plugin syntax
                    dockerImage = docker.build dockerImageName
                }
            }
        }
        stage('Setting up files'){
            steps{
                sshPut(remote: remote, from:'pythonAppDeploy/deployment.yml', into: '/home/brayanmarin/DevOps/miniProjects/4thWeek/pythonAppDeploy/')
                sshPut(remote: remote, from:'pythonAppDeploy/service.yml', into: '/home/brayanmarin/DevOps/miniProjects/4thWeek/pythonAppDeploy/')
                
            }
        }
        stage('Deploying API to Kubernetes'){
            steps{
                sshCommand(remote: remote, command: 'minikube kubectl -- apply -f /home/brayanmarin/DevOps/miniProjects/4thWeek/pythonAppDeploy/deployment.yml') 
                sshCommand(remote: remote, command: 'minikube kubectl -- apply -f /home/brayanmarin/DevOps/miniProjects/4thWeek/pythonAppDeploy/service.yml')
                // sshCommand(remote: remote, command: 'minikube kubectl -- apply -f /home/samir/MiniProject-4/hello-maven-deploy/service.yaml')
                sshCommand(remote: remote, command: 'minikube service python-app-service')
            }
        }
        
    
    }
}
