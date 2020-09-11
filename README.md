# Cloud Pak for Application Chapter GitOps - CI/CD Pipelines

This git repo contains all the artifacts needed to build a CI/CD pipeline and deploy a sample application for a variety of technologies.

# How to add to this Repo

## Target Project Structure

Please follow the file structure below when adding your assets.

```
Main_Repo_Route
│   README.md
│
└───Technology 1 (e.g. Node)
│   │   Readme.md
|   |
│   └───Pipelines
│   |   │   pipeline.yaml
│   |   │   ...
│   |   └───Tasks
│   |   │       task.yaml
│   |   │       ...
│   |   │───PipelineResources (optional)
│   |   │       pipelineResource.yaml
│   |   │       ...
│   |   │───PipelineRun
│   |   │       pipelineRun.yaml
│   |   │       ...
│   └───Triggers
│       │
│       └───TriggerTemplates
│       │       triggerTemplate.yaml
│       │       ...
│       │───TriggerBinding (optional)
│       │       triggerBinding.yaml
│       │       ...
│       │───EventListener
│       │       eventListener.yaml
│       │       ...
...
```

## README Structure

Each project should contain a README step by step guide how to take your code and get it running on an OpenShift cluster. Your README should follow the below format.

### **Intro**

This should be a very short intro to your sample application code. This should contain a path to your sample source code or a link to it if it's in another repo (ideally its part of your repo).

This is where you should also list any pre-reqs for the project. Where possible keep any installs to a minumum e.g. if you need a specific version of Java, have the Dockerfile compile it rather than the user's local machine.

Your pre reqs should also point to a how the user can install Tekton or the technology of choice. We are planning to have a section of this repo that you can link to that will be dedicated to how to install Tekton on Kubernetes and OpenShift (such as raw tekton or OpenShift Pipelines).

### **Pipelines**

This should contain instructions on how to build the pipeline.
E.g. Pipelines, Tasks and/or PipelineResources.

Privide an example of how to run the pipeline this was (e.g provide a working example of a PipelineRun to apply and not rely on a github webhook).

### **Triggers**

This should contain instructions on how to automatically trigger the pipeline.
E.g. TriggerTemplates, TriggerBindings, EventListeners

> The README should not contain TODOs (c'mon they never get done) and instead should be raised as a chore.
