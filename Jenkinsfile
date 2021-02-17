pipeline  {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/evramawad/hellowhale.git', branch:'master'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("evramawad/hellowhale:${env.BUILD_ID}")
                }
            }
        }
    
       stage('Deploying Docker Image to Nexus') {
                steps {
                    script {
                        docker.withRegistry('http://192.168.1.200:8082', 'c09bf387-73e4-48b3-982d-b74f75f97a1f') {
                        myapp.push()
                        }
                    }
                }
        }

    
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
