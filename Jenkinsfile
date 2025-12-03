pipeline{
    agent any
        stages{
            stage("Code"){
              steps{
                  echo("Cloning the Code")
                  git url:"https://github.com/prateek3697/my-notes-app.git", branch:"main"
              }  
            }
            stage("Build"){
                steps{
                  echo("Building the Image")
                  sh "docker build -t my-notes-app ."
              } 
            }
            stage("Push to DockerHub"){
                steps{
                  echo("Pushing the image to DockerHub")
                  withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                  sh "docker tag my-notes-app ${env.dockerHubUser}/my-notes-app:latest"
                  sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                  sh "docker push ${env.dockerHubUser}/my-notes-app:latest"
                  }
              } 
            }
            stage("Deploy"){
                steps{
                  echo("Deploying the Container to EC2")
                  sh "docker run -d -p 8000:8000 prateek3697/my-notes-app:latest"
              } 
            }
        }
}
