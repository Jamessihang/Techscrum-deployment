pipeline {
        agent any

        stages{   
            stage('Clone Repo') {
                 steps {
                    checkout scm
                   }
                }

            stage('update deployment file') {
	              steps{  
	                  script {
	                    sh "sed -i 's+527423341490.dkr.ecr.ap-southeast-2.amazonaws.com/techscrum.*+527423341490.dkr.ecr.ap-southeast-2.amazonaws.com/techscrum:${DOCKERTAG}+g' deployment.yaml"
	                  
	                  }
	                }
	            }                 
	                         
	        stage('Update deployment file') {
                steps{
				         script {
                     catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                       withCredentials([usernamePassword(credentialsId: 'Github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        //def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
                        sh "git config user.email jamessihang@hotmail.com"
                        sh "git config user.name Jamessihang"
                        //sh "git switch master"
                        sh "cat deployment.yml"
                        sh "sed -i 's+527423341490.dkr.ecr.ap-southeast-2.amazonaws.com/techscrum.*+527423341490.dkr.ecr.ap-southeast-2.amazonaws.com/techscrum:${DOCKERTAG}+g' deployment.yaml"
                        sh "cat deployment.yml"
                        sh "git add ."
                        sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                        sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/Techscrum-deployment.git HEAD:main"
                      }
                  }
            }
          }
        }
    }
}