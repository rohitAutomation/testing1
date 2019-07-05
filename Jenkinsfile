pipeline {

    agent any 

    stages {
        stage('merge master and push') {
 	/* This is not required, as of now.
    tools {
         maven 'M3'
         }*/
            steps {

               
               sh '''
                    
                    git fetch origin master
                    git merge master --no-ff  --no-edit
                    git push origin $GIT_BRANCH
                '''//sh  merge_master.sh $GIT_BRANCH  git pull origin $GIT_BRANCH

		  }
        }
        stage('Deploy Admin Job') { 
            steps {
                
                    echo "*** starting deployemnt in admin build ***"
                    build job: 'tesing', parameters: [[$class: 'StringParameterValue', name: 'test', value: "$GIT_BRANCH"]]
                
            }
        }
        stage('Delete temp PR Branch') { 
            steps {
                echo "*** removing temp PR branch ***"
                sh '''
                    git push \'https://github.com/rohitAutomation/testing1.git\' --delete --force $GIT_BRANCH
                '''
            }
        }

        stage('Wait for Admin server to be up') { 
            steps {
                echo "*** checking for admin server up or not ... ***"
               sh '''
                    sh server_up_status.sh
                '''
            }
        }
        stage('test') { 

            steps {
                echo "*** starting automation sanity run ***"
            }
        }

     }

}


