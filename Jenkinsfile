pipeline {

agent any

environment {

DOCKER_IMAGE='LakshmiPriyaYadav/poc1'

}

stages {

stage('Clone') {

steps {

git branch:'main',
url:'https://github.com/lakshmipriyayadav/Poc-1.git'

}

}

stage('Build') {

steps {

sh 'mvn clean verify'

}

}

stage('Sonar Scan') {

steps {

withSonarQubeEnv('SonarQube') {

sh 'mvn sonar:sonar'

}

}

}

stage('Dependency Check') {

steps {

dependencyCheck(
odcInstallation:'dependency-check',
additionalArguments:'--scan . --data /var/lib/jenkins/odc-data',
stopBuild:false
)

}

}

stage('Docker Build') {

steps {

sh '''
docker build -t $DOCKER_IMAGE .
'''

}

}

stage('Docker Push') {

steps {

withCredentials([
usernamePassword(
credentialsId:'dockerhub',
usernameVariable:'USER',
passwordVariable:'PASS'
)

]) {

sh '''
docker logout || true

docker login \
-u "$USER" \
-p "$PASS"

docker push $DOCKER_IMAGE
'''

}

}

}
}

}
