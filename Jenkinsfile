pipeline {
  agent any
  
   tools {nodejs "node"}
    
  stages {
    stage("Clone code from GitHub") {
            steps {
                script {
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github_credentials', url: 'https://github.com/jamsheerc/Deploy-NodeApp-to-AWS-EKS-using-Jenkins-Pipeline.git']])
                }
            }
        }
     
    stage('Node JS Build') {
      steps {
        sh 'npm install'
      }
    }
  
     stage('Build Node JS Docker Image') {
            steps {
                script {
                  sh 'docker build -t jamsheerc/node-app-1.0 .'
                }
            }
        }


        stage('Deploy Docker Image to DockerHub') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'dockerlogin', variable: 'dockerlogin')]) {
                    sh 'docker login -u jamsheerc -p ${dockerlogin}'
            }
            sh 'docker push jamsheerc/node-app-1.0'
        }
            }   
        }
         
     stage('Deploying Node App to Kubernetes') {
      steps {
        script {
          sh ('aws eks update-kubeconfig --name demo-ekscluster1 --region us-east-1')
          sh "kubectl apply -f node.yml"
        }
      }
    }

  }
}
