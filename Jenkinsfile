pipeline {
    agent  {
        label 'dind-agent'
    }
    stages {
        stage("Build") {
            steps {
                script {
                    sh "./mvnw clean install"
                }
            }
        }
        stage("Build image") {
            steps {
                script {
                    app = docker.build("eunoia0523/petclinic")
                }
            }
          }
        stage('Push to registry') {
            steps {
             script {
                        docker.withRegistry('https://asia.gcr.io', 'gcr:eunoia0523') {
                        app.push("${env.BUILD_NUMBER}")
                    }
                }
           }        
      }
   }
}
