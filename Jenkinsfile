pipeline {
agent any
tag = "1.0"
dockerHubUser="amanmishra9696@gmail.com"
containerName="insure-me"
httpPort="8081"
}
stages {
stage("code clone"){
steps {
checkout scmGit(branches: [[name: '*/main']], extensions: [], 
userRemoteConfigs: [[url: 'https://github.com/Aman9696n/asi-insurance.git']])
}
}
stage('Build') {
            steps {
                script {
                    def mavenHome = tool 'Maven3'
                    env.PATH = "${mavenHome}/bin:${env.PATH}"
                    sh 'mvn clean package'
                }
            }
        }
stage("Build Docker Image"){
steps{
sh "docker build -t ${dockerHubUser}/insure-me:${tag} ."
}
}
stage("push image to dockerhub"){
steps{
withCredentials([usernamePassword(credentialsId:
'dockerHubAccount', passwordVariable: 'dockerPassword', usernameVariable:
'dockerUser')]) {
sh "docker login -u $dockerUser -p $dockerPassword"
sh "docker push $dockerUser/$containerName:$tag"
}
}
}
stage("Docker container deployment"){
steps{
sh "docker rm $containerName -f"
sh "docker pull $dockerHubUser/$containerName:$tag"
sh "docker run -d --rm -p $httpPort:$httpPort --name $containerName$dockerHubUser/$containerName:$tag"
echo "Application started on port: ${httpPort} (http)"
}
}
}
}
