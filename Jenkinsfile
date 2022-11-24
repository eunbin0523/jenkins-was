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

        stage('K8S Manifest Update') {

            steps {

                git credentialsId: 'eunbin0523', 
                    url: 'https://github.com/eunbin0523/argo.git',
                    branch: 'master'

                sh "sed -i 's/petclinic:.*\$/petclinic:${env.BUILD_NUMBER}/g' petclinic-desvc.yaml"
                sh "git add petclinic-desvc.yaml"
                sh "git commit -m '[UPDATE] petclinic ${env.BUILD_NUMBER} image versioning'"

                withCredentials([gitUsernamePassword(credentialsId: 'eunbin0523')]) {
                    sh "git push -u origin master"
                }
            }
            post {
                    failure {
                    echo 'Update failure !'
                    }
                    success {
                    echo 'Update success !'
                    }
                }
           }        
      }
   }
}
