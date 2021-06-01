pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage("Deploy") {
          steps {
            script {
            echo 'Deploying to k8s cluster'
              try {
                  sh "kubectl apply -f k8s/httpd.yaml"
                  currentBuild.result = 'SUCCESS'
                } catch (Exception e) {
                    echo 'Exception occurred: ' + e.toString()
                }
                }
            }
        }
//         stage('Test-Deploy') {
//             steps {
//                 sh ""
//             }
//         }
    }
    post {
        success {
            echo 'Deployment done successfully'
        }
        failure {
            echo 'Deployment failed'
        }
    }
}
