pipeline{
    agent any
     environment {
        dockerImage='v1'
        registry = "sagarshirke2674/calc_mini"
        registryCredential = 'docker-login'
    }
    stages {
        stage('Pull GitHub Repository') {
            steps {
                git url: 'https://github.com/sagarshirke2674/Calc_mini.git', branch: 'master',
                 credentialsId: 'git-access-token'
            }
        }
        stage('Maven Build and Test'){
            steps {
                sh 'mvn clean install'
            }
        }
          stage('Building our image') {
            steps{
                
                script {
                    dockerImage = docker.build registry + ":$dockerImage"
                }
            
                
            }
        }
         stage('Deploy our image') {
            steps{
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()  
                    }
                }
            }
        }
        stage('Cleaning up') {
            steps{
                sh "docker rmi $registry:v1"
            }
        }
        stage('Ansible Deploy') {
             steps {
                  ansiblePlaybook colorized: true, disableHostKeyChecking: true, installation: 'Ansible', inventory: 'inventory', playbook: 'deploy.yml'
             }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}
