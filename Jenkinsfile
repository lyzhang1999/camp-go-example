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
        HARBOR_URL     = credentials('gitlab-token')
        IMAGE_PUSH_DESTINATION="${HARBOR_URL}/library/camp-go-example"
        GIT_COMMIT="${checkout (scm).GIT_COMMIT}"
        BUILD_IMAGE="${IMAGE_PUSH_DESTINATION}:${GIT_COMMIT}"
    }
    stages {
        stage('Build with Kaniko') {
            steps {
                // checkout scm
                container(name: 'kaniko', shell: '/busybox/sh') {
                    withEnv(['PATH+EXTRA=/busybox']) {
                        sh '''#!/busybox/sh
                            /kaniko/executor --force --context `pwd` --insecure --skip-tls-verify --cache=true --destination $BUILD_IMAGE
                        '''
                    }
                }
            }
        }
    }
}
