node{

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '4')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], 
pipelineTriggers([pollSCM('* * * * *')])])

def mavenHome = tool name: "maven3.8.3"

stage('CheckCode'){

git branch: 'development', credentialsId: '92641981-47f2-40ec-9b5a-e5ad7196f780', url:
'https://github.com/venkatrammgithub/maven-web-application.git'
}

stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

stage('Excecute SonarQube Report'){
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadArtifactRepoIntoNExus'){
sh "${mavenHome}/bin/mvn deploy"
}

stage('DeployIntoTomcatServer'){
sshagent(['ebb45879-24bb-4b46-a6ae-849d9f88cb67']){
 sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.126.161.169:/opt/apache-tomcat-9.0.56/webapps/"
}

}

}
