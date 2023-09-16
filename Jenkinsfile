pipeline {
    agent any

    environment{
        IMAGE_TAG="${BUILD_NUMBER}"
    }

    stages{
        stage('checkout'){
            steps{
                git branch: 'main', url : "https://github.com/kriteshsharma/jenkins.git"
            }
        }
        stage('Build Image '){
            steps{
                sh '''
                echo 'Building docker image'
                docker build -t krsharma/python-ci-cd:${BUILD_NUMBER} .
                '''
            }
        }
        stage('Push Image '){
            steps{
                sh '''
                echo 'Pushing docker image'
                docker push krsharma/python-ci-cd:${BUILD_NUMBER} 
                '''
            }
        }
        stage('Update manifest file'){
            steps{
                sh '''
                git config user.name "kriteshsharma"
                git conifg user.email "kritesh.bhojnagar@gmail.com"
                BUILD_NUMBER=${BUILD_NUMBER}
                sed -i '' "s/32/${BUILD_NUMBER}/g" python-jenkins-argocd-k8s/deploy/deploy.yaml
                cat python-jenkins-argocd-k8s/deploy/deploy.yaml
                git add python-jenkins-argocd-k8s/deploy/deploy.yaml
                git commit -m "updated manifest file using jenkins ci"
                git push https://github.com/kriteshsharma/jenkins.git HEAD:main
                '''
            }
        }
    }
}
