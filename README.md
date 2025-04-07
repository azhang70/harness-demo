# Todo List Example App for Harness.io

## Prerequisites 

> - Download [node.js](https://nodejs.org/en/)
> - Create an account on [Harness.io](https://www.harness.io/)
> - Create a repository on [GitHub](https://github.com/)
> - Install [Docker Desktop](https://docs.docker.com/desktop/) (with WSL if using Windows)
> - Create a local Kubernetes cluster using [minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/)

## Set Up

```sh
git clone https://github.com/azhang70/harness-demo.git
```

```sh
npm install
```

## Run

```sh
node app.js
```

Verify in http://localhost:8080

## CI/CD Using Harness
> - Click on 'Continuous Integration' and create a new project
> - Under 'Project-level Resources' click on 'Delegates' and install a [Harness Delegate](https://developer.harness.io/docs/platform/get-started/tutorials/install-delegate/#get-your-harness-account-id)
> - In the same section, click on 'Connectors' and create new Connectors for the Kubernetes cluster, DockerHub, and GitHub
> - Create a new pipeline with the following stages: ![image](https://github.com/user-attachments/assets/5a8d8642-093d-45d5-a80d-c4b2d969f87f)
> - In the top right corner, click on 'Triggers' and create a new trigger using the GitHub webhook. Use the GitHub Connector and provide the GitHub repository name. Use the push event.

 ### Test and Build Configuration
> - In the 'Infrastructure' tab, choose to run the builds on Kubernetes
> - Select the Linux OS
> - Select the Kubernetes connector and provide the namespace of the cluster
> - In the 'Execution' tab, create the following steps: ![image](https://github.com/user-attachments/assets/471b2fcd-f11d-45b6-8c67-6b0668a6a8bd)
> - In the 'Test' step, select Third-Party Artifact Registry as the Registry Type. Use the Harness Docker Connector to link the Container Registry. The remaining fields should be filled as such: ![image](https://github.com/user-attachments/assets/ab3aae98-b8ba-43fb-aed8-e57bd8898d1e)
> - In the 'Build and Push' step, select Third-Party Artifact Registry as the Registry Type and provide the Harness Docker Connector. Also input the docker repository name and any tags to be used.

### Deployment Configuration
> - In the 'Service' tab, add a new Service. Select Remote and Third-party Git Provider. Input the GitHub connector and GitHub repository name.
> - In the 'Environment' tab, add a new Environment. Select Remote and Third-party Git Provider. Input the GitHub connector,  GitHub repository name, and branch name.
> - In the 'Environment' tab, add new Infrastructure. Select Remote and Third-party Git Provider. Input the GitHub connector,  GitHub repository name, and branch name. Select Kubernetes as the Deployment Type.
> - In the 'Execution' tab, add a new step K8s Rollout Deploy.
> - In the 'Advanced' tab, define a failure strategy.

## Run the Pipeline
```sh
minikube start
```
Monitor the Delegates and wait for them to connect. This may take a few minutes.</br>
Push code to GitHub, then navigate to Harness and select 'Builds' to follow the pipeline's execution</br>
Once successful, run the following:
```sh
kubectl rollout restart deployment/todo-app
```
```sh
minikube service todo-app
```

## Tips
> - For the DockerHub Connector, use Username and Password as the authentication method. You may need to create a new Secret (personal access token) for this to work.
> - Make sure Docker Desktop is open before running 'minikube start'
> - If the pipeline is hanging, there is likely an issue with the Kubernetes cluster (check Delegate connectivity)
