pipeline {
  agent {
    kubernetes {
      label 'kubeagent'  // all your pods will be named with this prefix, followed by a unique id
      idleMinutes 5  // how long the pod will live after no jobs have run on it
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: shell
    image: ubuntu
    command:
    - sleep
    args:
    - infinity
  - name: builder
    image: gcr.io/kaniko-project/executor:debug
    imagePullPolicy: Always
    command:
    - /busybox/cat
    tty: true
'''
            // Can also wrap individual steps:
            // container('shell') {
            //     sh 'hostname'
            // }
      defaultContainer 'builder'
    }
  }
  stages {
    stage('Build Docker Image') {
      steps { 
          sh "/kaniko/executor --dockerfile `pwd`/Dockerfile --context `pwd` --destination=071690070779.dkr.ecr.ap-south-1.amazonaws.com/testreopk:lts"  // when we run docker in this step, we're running it via a shell on the docker build-pod container, 
      }
    }
  }
}