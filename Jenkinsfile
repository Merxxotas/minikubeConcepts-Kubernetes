def remote=[:]
remote.name = 'brayanmarin'
remote.host = '172.28.92.49'
remote.allowAnyHosts = true
pipeline{
    environment{
        dockerImageName = "merxxaz/helloworldjavamaven:v1.1.0"
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
                sshPut(remote: remote, from:'helloJavaMavenDeploy/deployment.yml', into: '/home/brayanmarin/DevOps/miniProjects/4thWeek/helloJavaMavenDeploy/')
                sshPut(remote: remote, from:'helloJavaMavenDeploy/service.yml', into: '/home/brayanmarin/DevOps/miniProjects/4thWeek/helloJavaMavenDeploy/')
                
            }
        }
        stage('Deploying API to Kubernetes'){
            steps{
                sshCommand(remote: remote, command: 'minikube kubectl -- apply -f /home/brayanmarin/DevOps/miniProjects/4thWeek/helloJavaMavenDeploy/deployment.yml') 
                sshCommand(remote: remote, command: 'minikube kubectl -- apply -f /home/brayanmarin/DevOps/miniProjects/4thWeek/helloJavaMavenDeploy/service.yml')
                sshCommand(remote: remote, command: 'minikube service hello-javamaven')
            }
        }
        
    
    }
}