# IBM GARAGE  |  Cloud Pak Acceleration Team  - CPAT
## Alfa Evolution BATCH Pipeline

## Configure your workspace
  ```bash
    oc new-project alfa   - Edit project name as needed
    oc project -q
    export NAMESPACE=$(oc project -q)
    echo "NAMESPACE set to $NAMESPACE"
```

## Create the pipeline artifacts
- Artifacts List:
```bash
  ./resources/batch-resources.yaml
  ./resources/template-git-secret.yaml
  ./tasks/batch-task-build.yaml
  ./tasks/batch-task-deploy.yaml
  ./pipeline/batch-pipeline.yaml
```

- Resources - under the resources folder
  - Define 2 resources: 
     - ``` batch-resources.yaml ```: defines source code git repo and the OCP deployment yaml files git repo
     - ``` template-git-secret.yaml ```: 
       - use this template to create a git secret resource: 
       
       ```  cp template-git-secret.yaml git-secret.yaml ```
       
       - ```git-secret.yaml ``` has been added to .gitignore to protect your credentials
       - REVIEW AND EDIT THE FILE AS NEEDED
- Tasks  - under the tasks folder
  - ``` batch-task-build.yaml ```: Define a task that build image, push the image to the registry of your choice, default to OCP internal registry
  - ``` batch-task-deploy.yaml ```: Define a task to the above mentioned image deploy to OCP. Deployment yaml files are defined on the ./deployment folder
- Pipeline
  - ``` batch-pipeline.yaml ```: run the two tasks defined above perform the deployment to OCP
  
```bash
    oc apply -f resources/batch-resources.yaml -n $NAMESPACE
    oc apply -f resources/git-secret.yaml -n $NAMESPACE
    oc secret link pipeline <YOUR_SECRET_NAMR>  (e.g. oc secret link pipeline git-infra-secret)
    tkn resources ls -n $NAMESPACE (shortcut: tkn res ls -n $NAMESPACE)

    oc apply -f tasks/ -n $NAMESPACE
    tkn task ls -n $NAMESPACE

    oc apply -f pipelines/ -n $NAMESPACE
    tkn pipeline ls -n $NAMESPACE
```
NOTE: Do not forget to link your git secret to the OOO OCP Service Account called ``` pipeline```

# Run the pipeline
```bash
    tkn pipeline start batch-pipeline --showlog \
    -r batch-git=batch-git \
    -r batch-git-tekton=batch-git-tekton 
    -s pipeline \
    -n $NAMESPACE
```
# Debug 
```bash
    oc get imagestreams -n $NAMESPACE

    tkn pipelinerun ls -n $NAMESPACE
    tkn pipelinerun logs <PIPELINE_RUN_NAME>

    tkn taskrun ls -n $NAMESPACE
```

#Create Git Webhook

- This is only possible if your OpenShift cluster is accessible from the the github server

```bash
oc apply -f .triggers/batch-triggers.yaml -n $NAMESPACE
oc expose svc el-batch
oc get route el-batch -o jsonpath='{.spec.host}'

export GIT_REPO_NAME=BATCH
export GIT_REPO_OWNER=alfa-tsp/poc
export GIT_USERNAME=<YOUR_USER_NAME>  (e.g.; Omar-Gaye)
export GIT_TOKEN=<YOUR_GIT_TOKEN>
export GIT_WEBHOOK_URL=$(oc get route el-batch -o jsonpath='{.spec.host}')

--- Create webhook manually or by using the following GIT API call

curl -v -X POST -u $GIT_USERNAME:$GIT_TOKEN \
-d "{\"name\": \"web\",\"active\": true,\"events\": [\"push\"],\"config\": {\"url\": \"https://$GIT_WEBHOOK_URL\",\"content_type\": \"json\",\"insecure_ssl\": \"0\"}}" \
-L https://api.us-south.git.cloud.ibm.com/repos/$GIT_REPO_OWNER/$GIT_REPO_NAME/hooks
```
Notes:

  - Make a change on the Code repository, commit and check in change
  - Verify that Github sent the WebHook to the event listener, and that the Pipeline runs in OpenShift Console
