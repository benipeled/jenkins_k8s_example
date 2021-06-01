pipeline {
    agent any
    environment {
        K8S_CONFIG_PATH = '/var/jenkins_home'
    // K8S_ENV = '${params.K8S_ENV}'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Setup parameters') {
            steps {
                script {
                    properties([
                        parameters([
                            choice(
                                choices: ['PROD', 'DEV', 'QA'],
                                name: 'K8S_ENV'
                            ),
                        ])
                    ])
                }
            }
        }
        stage("Deploy") {
          steps {
            script {
            echo "Deploy to ${params.K8S_ENV} cluster"
              try {
                  sh "kubectl --kubeconfig=${K8S_CONFIG_PATH}/.kube/${params.K8S_ENV} delete -f k8s/httpd.yaml"
                  sh "kubectl --kubeconfig=${K8S_CONFIG_PATH}/.kube/${params.K8S_ENV} apply -f k8s/httpd.yaml"
                  currentBuild.result = 'SUCCESS'
                } catch (Exception e) {
                    echo 'Exception occurred: ' + e.toString()
                }
                }
            }
        }
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
