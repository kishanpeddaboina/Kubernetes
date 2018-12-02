node{
    stage('Scm Checkout'){
    git credentialsId: 'git', url: 'https://github.com/kishanpeddaboina/Docker'
}
    stage('Mvn Package'){
    def mvnHome = tool name: 'maven', type: 'maven'
def mvnCMD = "${mvnHome}/bin/mvn"
    sh "${mvnCMD} clean package"
}
     stage('Build Docker'){
     sh 'docker build -t kishanpeddaboina/my-app:${BUILD_NUMBER} .'
}
    stage('Push Docker Image'){
    withCredentials([string(credentialsId: 'docker', variable: 'docker')]) {
     sh "docker login -u kishanpeddaboina -p ${docker}"
}
     sh 'docker push kishanpeddaboina/my-app:${BUILD_NUMBER}'
  

}

stage('Remove Old Containers'){
    sshagent(['dev-staging']) {
      try{
        def sshCmd = 'ssh -o StrictHostKeyChecking=no ubuntu@52.34.244.220'
        def dockerRM = 'docker rm -f my-kishan'
        sh "${sshCmd} ${dockerRM}"
      }catch(error){

      }
    }
  }

   stage('Runcontainer on Kubernetes server'){
  sh "kubectl apply -f deployment.yml"
 sh "ssh naveen@13.57.37.134"
}

}


}


