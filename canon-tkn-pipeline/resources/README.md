# IBM GARAGE | Cloud Pak Acceleration Team - CPAT
## Alfa Evolution Containerization POC pipeline - BATCH
 

## Pipeline Resource
```bash
batch-resources.yaml
git-secret.yaml
```

        
## Secret Template
  - *template-git-secret.yaml*
    1. cp template-git-secret.yaml git-secret.yaml
    2. Replace the metadata/name field
    3. The annotation -
    
       ```yaml
       annotations:
         tekton.dev/git-0: https://us-south.git.cloud.ibm.com
       ```
       Is necessary for accessing any private IBM repo
    4. Replace the username field with your IBM e-mail address
    5. Provide a GitHub personal access token in the password field **( MUST HAVE READ:PACKAGES ACCESS )**
    
## NOTE: to protect your credentials, git-secret.yaml has already been added to the .gitignore file.
        
        
## Create the Resources
```bash
oc apply -f batch-resources.yaml
oc apply -f git-secret.yaml
```

## ServiceAccount
  - Use the OOB OCP ServiceAccount called pipeline. The command below will link your new secret.
  
    ```bash
    oc secret link pipeline <secret-name>
    example 
    oc secret link pipeline git-infra-secret
    ```
