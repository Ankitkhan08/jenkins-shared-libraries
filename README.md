# Jenkins Shared Libraries

Reusable Groovy functions for Jenkins pipelines — 
write the logic once, use it across multiple pipelines.

## What this is
Jenkins Shared Libraries let you extract common pipeline 
steps into centralised functions. Instead of copying the 
same Docker build and push logic into every Jenkinsfile, 
you call a shared function by name.

## Available Functions

### `clone.groovy`
Clones a GitHub repository inside a pipeline stage.
```groovy
clone("https://github.com/your-repo.git", "main")
```

### `docker_build.groovy`
Builds a Docker image from the current workspace.
```groovy
docker_build("image-name", "tag")
```

### `docker_push.groovy`
Pushes a Docker image to a container registry.
```groovy
docker_push("image-name", "tag", "registry-url")
```

### `hello.groovy`
Basic test function used to verify shared library 
is loaded correctly in Jenkins.
```groovy
hello("World")
```

## How to use in Jenkins
1. Go to **Manage Jenkins → Configure System → 
   Global Pipeline Libraries**
2. Add this repo as a shared library
3. In your Jenkinsfile:
```groovy
@Library('jenkins-shared-libraries') _

pipeline {
    agent any
    stages {
        stage('Clone') {
            steps { clone("https://github.com/your-repo.git", "main") }
        }
        stage('Build') {
            steps { docker_build("myapp", "latest") }
        }
        stage('Push') {
            steps { docker_push("myapp", "latest", "registry-url") }
        }
    }
}
```

## Why shared libraries matter
Without shared libraries, every pipeline duplicates the 
same logic. One change (e.g., updating a registry URL) 
means editing every Jenkinsfile. With shared libraries, 
you change it once and every pipeline gets the update.
