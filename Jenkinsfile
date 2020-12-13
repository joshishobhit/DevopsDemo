pipeline {
  agent any
  stages {    
    stage('Build & Analysis') {
      steps {
        sh '''
           sudo dotnet /opt/sonarscanner_dotnet/SonarScanner.MSBuild.dll begin /k:"dotnet_demo" /d:sonar.login="ed21c0d4272a7631e40713a91c884e902ca67f9e"
           sudo dotnet build "DevopsDemo.csproj" -c Release -o /app/build
           sudo dotnet /opt/sonarscanner_dotnet/SonarScanner.MSBuild.dll end /d:sonar.login="ed21c0d4272a7631e40713a91c884e902ca67f9e"

           '''
      }   
    }
    stage('building docker image') {
      steps {
        sh 'sudo docker build -t xlshobhit/devopsdemo:$BUILD_NUMBER .'
      }   
    }
    stage('logging into docker hub') {
      steps {
        sh 'docker login --username="xlshobhit" --password="Xpertladr@123"'
      }   
    }
    stage('push Image to HUB') {
      steps {
        sh 'sudo docker push xlshobhit/devopsdemo:$BUILD_NUMBER'
      }   
    }  
    steps {
        sh '''
            sudo docker rm -f shobhit-c1
            docker run -itd --name=shobhit-c1 -p 9500:80 xlshobhit/devopsdemo:$BUILD_NUMBER            
           '''
      }   
    }
}
post {
    failure {
        mail to: 'joshi.shobhit@gmail.com',
             subject: "Failed Pipeline: ${BUILD_NUMBER}",
             body: "Something is wrong with ${env.BUILD_URL}"
    }
     success {
        mail to: 'joshi.shobhit@gmail.com',
             subject: "successful Pipeline:  ${env.BUILD_NUMBER}",
             body: "Your pipeline is success ${env.BUILD_URL}"
    }
}
}
