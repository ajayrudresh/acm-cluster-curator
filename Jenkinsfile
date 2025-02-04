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
                   if [[ "$MASTER_LATEST_COMMIT_FILE" == "acm-register-cluster/values"* ]]
                   then   
                     git show HEAD:$MASTER_LATEST_COMMIT_FILE > /tmp/latest_commit_file
                     git show HEAD~1:$MASTER_LATEST_COMMIT_FILE > /tmp/previous_commit_file
                     UPGRADE_CONFIG_DIFF=$(diff /tmp/latest_commit_file /tmp/previous_commit_file | grep upgrade || true)
                     if [[ "$UPGRADE_CONFIG_DIFF" == "> upgrade:" ]]
                     then
                       CLUSTER_ID=$(echo ${MASTER_LATEST_COMMIT_FILE} | sed 's/acm-register-cluster\/values-//' | sed 's/.yaml//')
                       ARGO_CD_APPLICATION=cluster-${CLUSTER_ID}
                       oc patch  application $ARGO_CD_APPLICATION --type=merge -p \
                        '{
                          "operation": {
                              "sync": {
                                  "prune": true,
                                  "resources": [
                                      {
                                          "group": "cluster.open-cluster-management.io",
                                          "kind": "ClusterCurator",
                                          "name": "$CLUSTER_ID",
                                          "namespace": "$CLUSTER_ID"
                                      }
                                  ]
                              }
                          }
                        }'
                     else
                       echo "Upgrade configuration is not removed in the values file. Hence no need to prune the cluster curator"
                     fi
                   else
                     echo "Values file is not changed. Hence no need to prune the cluster curator"
                   fi
                 else
                   echo "Current Branch is $CURRENT_GIT_BRANCH (not a master). Hence no need to prune the cluster curator"
                 fi
                 '''
               }
          }
       }
   }
}
