pipeline {
  agent any
  stages {
    stage('Restore Project') {
      steps {
        sh 'dotnet restore DevopsDemo.csproj'
      }
    }
    stage('Dotnet Build') {
      steps {
        sh 'sudo dotnet build "DevopsDemo.csproj" -c Release -o /app/build'
      }
    }
    stage('building docker image from docker file by tagging') {
      steps {
        sh 'docker build -t xlshobhit/devopsdemo:$BUILD_NUMBER .'
      }   
    }
    stage('push Image to HUB') {
      steps {
        sh 'docker push xlshobhit/devopsdemo:$BUILD_NUMBER'
      }   
    }
    // stage('deploying the docker image into EC2 instance and run the container') {
    //   steps {
    //     sh 'ansible-playbook deploy.yml --extra-vars="buildNumber=$BUILD_NUMBER"'
    //   }   
    // }  
}
post {
    failure {
        mail to: 'shobhit@xpertladr.com',
             subject: "Failed Pipeline: ${BUILD_NUMBER}",
             body: "Something is wrong with ${env.BUILD_URL}"
    }
     success {
        mail to: 'shobhit@xpertladr.com',
             subject: "successful Pipeline:  ${env.BUILD_NUMBER}",
             body: "Your pipeline is success ${env.BUILD_URL}"
    }
}
}
