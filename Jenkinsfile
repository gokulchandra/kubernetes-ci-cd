node {
    
    checkout scm

    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "hello-kenzan"
    registryHost = "127.0.0.1:30400/"
    imageName = "${registryHost}${appName}:${tag}"
    env.BUILDIMG=imageName
    this.echo "Inside class!!"
    docker.image('docker:18.04.0-ce-dind').inside  {
        stage "Build"
            this.echo "Inside class"
            def customImage = docker.build("applications/hello-kenzan", "./applications/hello-kenzan/Dockerfile") 
        
        stage "Push"
            withDockerRegistry(registryHost) {
                customImage.push(imageName)
            }

        stage "Deploy"

            sh "sed 's#127.0.0.1:30400/hello-kenzan:latest#'$BUILDIMG'#' applications/hello-kenzan/k8s/deployment.yaml | kubectl apply -f -"
            sh "kubectl rollout status deployment/hello-kenzan"
    }    
}
