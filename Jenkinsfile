pipeline {
    agent any
    parameters {
        choice(name: 'ENV', choices: ['dev', 'test', 'prod',"release"])
    } 
    stages {
        stage('build') {
            steps {
                echo 'build'
                script{
                    if (params.ENV == "release") {
                        withCredentials([usernamePassword(credentialsId: 'DockerHub-Account', usernameVariable: 'USERNAME', passwordVariable: 'Passw')]) {
                            sh '''
                                docker login -u ${USERNAME} -p ${Passw}
                                docker build -t mustafaessam/backendsimpleapp:v55 .
                                docker push mustafaessam/backendsimpleapp:v55
                                echo ${BUILD_NUMBER} > ../build.txt
                            '''
                        }
                    }
                    else {
                        echo "user choosed ${params.ENV}"
                    }
                }
            }
        }
        stage('deploy') {
            steps {
                echo 'deploy'
                script {
                    if (params.ENV == "dev" || params.ENV == "test" || params.ENV == "prod") {
                        # withCredentials([file(credentialsId: 'slave-kubeconfige', variable: 'KUBECONFIG_To_Slave')])
                        
                        {
                            sh '''
                            kubectl apply -f /home/ec2-user/workspace/backend/LoadBalancer.yaml
                            kubectl apply -f /home/ec2-user/workspace/backend/deployment.yaml
                            
                                #    export BUILD_NUMBER=$(cat ../build.txt)
                                #   mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                                #  cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                                #  rm -f Deployment/deploy.yaml.tmp
                                # kubectl apply -f Deployment --kubeconfig ${KUBECONFIG_To_Slave} -n ${ENV}
                            '''
                        }
                    }
                }
            }
        }

        stage("Display"){
            steps{
                echo "============= Displaying choices . . . "
                
                script{
                     def choicesd = ['1] dev', '2] test', '3] prod',"4] release"]
                     for (ans in choicesd){
                            echo "${ans}"
                     }
                }
            }
        }

    }
}
