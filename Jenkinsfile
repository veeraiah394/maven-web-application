node{
 properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], 
pipelineTriggers([pollSCM('* * * * *')])])

def mavenhome = tool name: "maven3.8.4"
stage("checkout code"){
git branch: 'development', credentialsId: '5d0ed8ed-6853-4a75-ae86-e1aaa5c305ff', url: 'https://github.com/veeraiah394/maven-web-application.git'
}

stage("build"){
sh "${mavenhome}/bin/mvn clean package"
}

stage("excute sonarqube report"){
sh "${mavenhome}/bin/mvn sonar:sonar"
}
stage("upload artifacts"){
sh "${mavenhome}/bin/mvn deploy"
}
stage('deploy application into tomcat server'){
sshagent(['f484da2e-10d2-4ff6-b2a7-4bc22a924410']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.126.161.160:/opt/apache-tomcat-9.0.58/webapps/"

}

}
}
