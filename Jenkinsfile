pipeline {
        agent any

        environment {
              AWS_CRED = "awsCredntials"
            }
        stages{   
            stage('Clone Repo') {
                 steps {
                    checkout scm
                   }
                }

            stage('update deployment file'){
	              steps{  
	                  script {
	                    sh "sed -i 's+527423341490.dkr.ecr.ap-southeast-2.amazonaws.com/techscrum.*+527423341490.dkr.ecr.ap-southeast-2.amazonaws.com/techscrum:${DOCKERTAG}+g' deployment.yaml"
	                  
	                  }
	                }
	            }                 
	                         
	        stage('eks update-kubeconfig'){
	              steps {
	                  withAWS(credentials: AWS_CRED, region: 'ap-southeast-2'){
	                    sh 'aws eks --region ap-southeast-2 update-kubeconfig --name techscrum'}
	                }
	            }
	        
	        stage('eks deployment update'){
	               steps{
	                   withAWS(credentials: AWS_CRED, region: 'ap-southeast-2'){                    
	                    sh 'kubectl apply -f deployment.yaml'	                   
	                   }
	                }
	            } 
        }
    }