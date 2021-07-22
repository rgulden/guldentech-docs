# Creating a test, build and deploy pipeline with Rancher

Please review the Rancher pipeline documentation page here (https://rancher.com/docs/rancher/v2.0-v2.4/en/pipelines/) for creating pipelines.

But the essential part is making sure, on the rancher console you follow the steps outlined after clicking "Configure Repos".

![pipeline](../_media/pipeline.png)

If its your first time, it will ask you to setup your connection to github. Follow the steps it says.

## Rancher Pipelie YAML

Your repo will need a .rancher-pipeline.yaml file which will faciliate the pipeline. 

Below is a basic Build and deploy example.

```yaml
stages:
- name: Publish
  steps:
  - publishImageConfig:
      dockerfilePath: ./Dockerfile
      buildContext: .
      tag: image-name:v1.0
    env:
      PLUGIN_MTU: "1230"
- name: Deploy
  steps:
  - applyYamlConfig:
      path: ./deploy.yaml
```

Above defines it will first build a image from the "Dockerfile" file at the root of your repo, push it to the local registry with the name image-name:v1.0. Following that, it will run kubectl apply -f deploy.yaml. 

!> Make sure your namespace is defined in your deploy file.

## Deploy config settings

In your deploy.yaml file your will need to add three special sections. 

1. Pipeline secrets
2. Image name
3. Git Commit

Example:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
	  imagePullSecrets:           # <----- 1.
      - name: pipeline-docker-registry
      containers:
      - name: nginx
        image: ${CICD_IMAGE}:v1.0 # <----- 2.
		env:
        - name: CICD_GIT_COMMIT   # <----- 3.
          value: "${CICD_GIT_COMMIT}"
        ports:
        - containerPort: 80
```

## Alerts for failures

If you would like, we can configure alerts to the pipeline yaml file that will send you a email on conditions.

```yaml
stages: {}
notification:
  recipients:
  - recipient: "test@example.com"
    notifier: "c-hmqnj:n-xv76m"
  # Select which statuses you want the notification to be sent
  condition: ["Failed", "Success", "Changed"]
  # Ability to override the default message (Optional)
  message: "my-message"

```