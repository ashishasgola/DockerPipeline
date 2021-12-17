def containerName="docker-pipeline"
def tag="latest"
def dockerHubUser="ashishasgola"
def httpPort="8090"

node {

    stage('Checkout') {
        checkout scm
    }

    stage('Build'){
        sh "mvn clean install"
    }

    stage("Image Prune"){
         sh "docker image prune -f"
    }

    stage('Image Build'){
        sh "docker build -t $containerName:$tag  -t $containerName --pull --no-cache ."
        echo "Image build complete"
    }

    stage('Push to Docker Registry'){
        withCredentials([usernamePassword(credentialsId: 'e936f288-9922-493f-93ab-1cea5505c9a4', usernameVariable: 'dockerUser', passwordVariable: 'dockerPassword')]) {
            sh "docker login -u $dockerUser -p $dockerPassword"
            sh "docker tag $containerName:$tag $dockerUser/$containerName:$tag"
            sh "docker push $dockerUser/$containerName:$tag"
            echo "Image push complete"
        }
    }

    stage('Run App'){
       /* sh "docker rm $containerName -f"
        sh "docker pull $dockerHubUser/$containerName"
        sh "docker run -d --rm -p $httpPort:$httpPort --name $containerName $dockerHubUser/$containerName:$tag"
        echo "Application started on port: ${httpPort} (http)"
        */
    }

}
