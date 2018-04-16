node {
    
    checkout scm

    tag = "latest"
    appName = "hello-kenzan"
    imageName = "{appName}:${tag}"
    
    docker.image('docker:18.04.0-ce-dind').inside  {
        def customImage = docker.build("applications/hello-kenzan", "./applications/hello-kenzan/Dockerfile") 
        this.echo imageName
    }
           
}
