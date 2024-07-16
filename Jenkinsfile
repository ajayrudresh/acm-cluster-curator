pipeline {
   agent any
   stages {
      stage('clone repo') {
          steps {
              script {
                 sh '''
                 git clone -b main https://github.com/ajayrudresh/acm-cluster-curator.git
                 cd acm-cluster-curator
                 echo "Fetching current branch name"
                 CURRENT_GIT_BRANCH=$(git branch | sed 's/* //g')
                 if [[ $CURRENT_GIT_BRANCH -eq master ]]
                 then
                   echo "Current Branch is $CURRENT_GIT_BRANCH."
                 '''
               }
          }
       }
   }
}
