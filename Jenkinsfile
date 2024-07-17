pipeline {
   agent any
   stages {
      stage('Prune Cluster Curator in ArgoCD') {
          steps {
              script {
                 sh '''
                 rm -rf acm-cluster-curator
                 git clone -b main https://github.com/ajayrudresh/acm-cluster-curator.git
                 cd acm-cluster-curator
                 echo "Fetching current branch name"
                 CURRENT_GIT_BRANCH=$(git branch | sed 's/* //g')
                 if [[ "$CURRENT_GIT_BRANCH" == "main" ]]
                 then
                   echo "Current Branch is $CURRENT_GIT_BRANCH."
                   echo "Check if values file is changed in the last commit"
                   MASTER_LATEST_COMMIT_FILE=$(git show HEAD  --name-only  --pretty="")
                   if [[ "$MASTER_LATEST_COMMIT_FILE" == "values" ]]
                   then
                     git show HEAD:$MASTER_LATEST_COMMIT_FILE > /tmp/latest_commit_file
                     git show HEAD~1:$MASTER_LATEST_COMMIT_FILE > /tmp/previous_commit_file
                     diff /tmp/latest_commit_file /tmp/previous_commit_file
                   else
                     echo "Values file is not changed. Hence no need to Prune the Cluster Curator"
                 else
                   echo "Current Branch is $CURRENT_GIT_BRANCH (not a master). Hence no need to Prune the Cluster Curator"
                 fi
                 '''
               }
          }
       }
   }
}
