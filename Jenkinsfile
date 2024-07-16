pipeline {
   agent any
   stages {
      stage('clone repo') {
          steps {
              script {
                 sh '''
                 rm -rf acm-cluster-curator
                 git clone -b main https://github.com/ajayrudresh/acm-cluster-curator.git
                 cd acm-cluster-curator
                 echo "Fetching current branch name"
                 CURRENT_GIT_BRANCH=$(git branch | sed 's/* //g')
                 if [[ $CURRENT_GIT_BRANCH -eq main ]]
                 then
                   echo "Current Branch is $CURRENT_GIT_BRANCH."
                   echo "Check if values file is changed in the last commit"
                   MASTER_LATEST_COMMIT_VALUES=$(git show HEAD  --name-only  --pretty="" | grep values)
                   MASTER_PREVIOUS_COMMIT_VALUES=$(git show HEAD~1  --name-only  --pretty="" | grep values)
                 fi
                 '''
               }
          }
       }
   }
}
