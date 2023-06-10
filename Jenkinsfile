pipeline {
    agent {
        kubernetes {
            defaultContainer 'kaniko'
            yaml """
kind: Pod
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:v1.6.0-debug
    imagePullPolicy: Always
    command:
    - sleep
    args:
    - 99d
    volumeMounts:
      - name: jenkins-docker-cfg
        mountPath: /kaniko/.docker
  volumes:
  - name: jenkins-docker-cfg
    projected:
      sources:
      - secret:
          name: regcred
          items:
            - key: .dockerconfigjson
              path: config.json
"""
        }
    }
    environment {
        IMAGE_PUSH_DESTINATION="lyzhang1999/camp-go-example"
    }
    stages {
        stage('Build with Kaniko') {
            steps {
                // checkout scm
                container(name: 'kaniko', shell: '/busybox/sh') {
                    withEnv(['PATH+EXTRA=/busybox']) {
                        sh '''#!/busybox/sh
                            /kaniko/executor --force --context `pwd` --insecure --skip-tls-verify --cache=true --destination $IMAGE_PUSH_DESTINATION
                        '''
                    }
                }
            }
        }
    }
}