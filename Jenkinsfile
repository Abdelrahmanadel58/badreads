pipeline {
    agent { label 'slave' }
    stages {
        stage('Build-backend') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-login', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                        sh """
                            docker login -u '${USERNAME}' -p '${PASSWORD}'
                            docker build -t abdelrahman58/badreadsbackend-$BRANCH_NAME-$BUILD_NUMBER ./badreads-frontend
                            docker push abdelrahman58/badreadsbackend-$BRANCH_NAME-$BUILD_NUMBER
                        """
                     }   
                    }

                }
             }
        stage('Build-frontend') {
            steps {
                script {
                        sh """
                            docker build -t abdelrahman58/badreadsfrontend-$BRANCH_NAME-$BUILD_NUMBER badreads-frontend/
                            docker push abdelrahman58/badreadsfrontend-$BRANCH_NAME-$BUILD_NUMBER
                        """
                   }

                }
             }
        stage('Deploy-application') {
            steps {
               withCredentials([file(credentialsId: 'k8s', variable: 'Secretfile')]) {
                script {
                        sh """
                            cat k8s/backend.yaml | envsubst > k8s/back-end.yaml
                            cat k8s/frontend.yaml | envsubst > k8s/front-end.yaml
                            kubectl apply -f k8s/ --kubeconfig=$Secretfile
                        """
                    }
                }
            }
         }
        stage('remove local images'){
            steps{
                sh "docker rmi -f abdelrahman58/badreadsfrontend-$BRANCH_NAME-$BUILD_NUMBER"
                sh "docker rmi -f  abdelrahman58/badreadsbackend-$BRANCH_NAME-$BUILD_NUMBER"
            }
        }

         stage ('cleanup'){
            steps{
                cleanWs()
            }
        }

      }
    }
