pipeline {
  agent {
    docker {
      image 'avasekho/jenkins:jenkins-agent-1.4'
    }
  }
  environment {
    HOST_CREDS = credentials('55a9dfb4-d756-446f-94d5-88c726378cd8')
    DOCKERHUB_CREDS = credentials('34d2a98c-ee5a-4a65-939e-44a8a9c18d97')
  }
    stages {

      stage ('git clone') {
        steps {
          git 'https://github.com/boxfuse/boxfuse-sample-java-war-hello.git'
      }
      }
      stage ('build war') {
        steps {
          sh 'mvn package'
          sh 'mkdir -p ./hwk/ && cp /var/lib/jenkins/workspace/assembly_pipe/target/hello-1.0.war ./hwk/'
      }
      }
      stage ('connect to host') {
        steps {
          sh 'scp root@178.154.198.133:/dockerfiles/Dockerfile ./hwk/'
        }
      }
      stage ('build docker') {
        steps {
          sh 'cd ./hwk/ && docker build -t boxfuze-app .'
          sh 'docker login -u $DOCKERHUB_CREDS_USR -p $DOCKERHUB_CREDS_PSW'
          sh 'docker tag boxfuze-app avasekho/jenkins:boxfuze-app && docker push avasekho/jenkins:boxfuze-app'
      }
      }
      }
  }