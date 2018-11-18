node {

    checkout scm

    env.DOCKER_API_VERSION="1.23"
    
    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "hello-kenzan"
    registryHost = "10.10.30.5:30400/"
    imageName = "${registryHost}${appName}:${tag}"
    env.BUILDIMG=imageName

    stage "Build"
    
        sh "sudo docker build -t ${imageName} -f applications/hello-kenzan/Dockerfile applications/hello-kenzan"
    
    stage "Push"

        sh "sudo docker push ${imageName}"

    stage "Deploy"

        kubernetesDeploy configs: "applications/${appName}/k8s/*.yaml", kubeconfigId: 'kenzan_kubeconfig'

}
