pipeline {
  agent any
  stages {
    stage('Restore Project') {
      steps {
        sh 'dotnet restore DevopsDemo.csproj'
      }
    }
    stage('Build & Analysis') {
      steps {
        sh '''
           sudo dotnet /opt/sonarscanner_dotnet/SonarScanner.MSBuild.dll begin /k:"dotnet_demo" /d:sonar.login="ed21c0d4272a7631e40713a91c884e902ca67f9e"
           sudo dotnet build "DevopsDemo.csproj" -c Release -o /app/build
           sudo dotnet /opt/sonarscanner_dotnet/SonarScanner.MSBuild.dll end /d:sonar.login="ed21c0d4272a7631e40713a91c884e902ca67f9e"

           '''
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
