pipeline {
    agent{
        label 'k8s-master'
    }
    stages {
        stage("cloning git"){
            steps{
                 git branch: 'main', url: 'https://github.com/Shudhoo/devops-evaluation-project-1.1.git'
            
            }
        }
        stage(" Docker Build & Push"){
            steps{
                withCredentials([usernamePassword(credentialsId: 'Docker', passwordVariable: 'Password', usernameVariable: 'Shudhodhan')]) {
                    dir('/home/ubuntu/workspace/test-job') {
                        sh '''OLD_CONTAINER=$(sudo docker ps --filter "publish=85" --format "{{.ID}}")
                              sudo docker rm $OLD_CONTAINER --force
                              sudo docker build --file Dockerfile --tag shudhodhan/testrepo:${BUILD_NUMBER} .
                              echo $Password | docker login -u $Shudhodhan --password-stdin
                              sudo docker push shudhodhan/testrepo:${BUILD_NUMBER} 
                              sudo docker run -itd -p 85:80 --name myapp-${BUILD_NUMBER} shudhodhan/testrepo:${BUILD_NUMBER}'''
                    }
                }
            }
        }
        stage("Trigger test-job1"){
            steps {
                build job: 'test-job1',
                    parameters: [string(name: 'IMAGE_TAG', value: "${BUILD_NUMBER}")],
                    wait: false
            }
        }
    }
    
}