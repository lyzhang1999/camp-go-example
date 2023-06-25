pipeline {
    agent none
    stages {

//         stage('Unit Test') {
//             agent {
//                 kubernetes {
//                     defaultContainer 'golang'
//                     yaml """
// kind: Pod
// spec:
//   containers:
//   - name: golang
//     image: golang:alpine
//     imagePullPolicy: Always
//     command:
//     - sleep
//     args:
//     - 99d
// """
//                 }
//             }
//             steps {
//                 // script {
//                 //     properties([pipelineTriggers([pollSCM('* * * * *')])])
//                 // }
//                 container(name: 'golang', shell: '/bin/sh') {
//                     sh '''#!/bin/sh
//                         go test ./... -coverprofile cover.out
//                     '''
//                 }
//             }
//         }

//         stage('Scan Code with Sonarqube') {
//             agent {
//                 kubernetes {
//                     defaultContainer 'sonar-scanner'
//                     yaml """
// kind: Pod
// spec:
//   containers:
//   - name: sonar-scanner
//     image: sonarsource/sonar-scanner-cli@sha256:e028b6fd811f0184a3ff7f223a66908c3c359fa559c97fa2ee87042c2b540415
//     imagePullPolicy: Always
//     command:
//     - sleep
//     args:
//     - 99d
// """
//                 }
//             }

//             environment {
//                 HARBOR_URL     = credentials('harbor-url')
//                 SONAR_TOKEN     = credentials('sonarqube-token')
//                 SONAR_SCANNER_OPTS = "-Dsonar.projectKey=camp-go-example -Dsonar.token=${SONAR_TOKEN}"
//                 SONAR_HOST_URL = "http://sonar${HARBOR_URL.replaceAll('harbor','')}."
//             }

//             steps {
//                 // script {
//                 //     properties([pipelineTriggers([pollSCM('* * * * *')])])
//                 // }
//                 container(name: 'sonar-scanner', shell: '/bin/sh') {
//                     withSonarQubeEnv('camp-go-example') {
//                         sh '''#!/bin/sh
//                             sonar-scanner
//                         '''
//                     }
//                     timeout(time: 1, unit: 'HOURS') {
//                         waitForQualityGate abortPipeline: true
//                     }
//                 }
//             }
//         }

//         stage('Build with Kaniko') {
//             agent {
//                 kubernetes {
//                     defaultContainer 'kaniko'
//                     yaml """
// kind: Pod
// spec:
//   containers:
//   - name: kaniko
//     image: gcr.io/kaniko-project/executor:v1.6.0-debug
//     imagePullPolicy: Always
//     command:
//     - sleep
//     args:
//     - 99d
//     volumeMounts:
//       - name: jenkins-docker-cfg
//         mountPath: /kaniko/.docker
//   volumes:
//   - name: jenkins-docker-cfg
//     projected:
//       sources:
//       - secret:
//           name: regcred
//           items:
//             - key: .dockerconfigjson
//               path: config.json
// """
//                 }
//             }

//             environment {
//                 HARBOR_URL     = credentials('harbor-url')
//                 HARBOR_REPOSITORY     = credentials('harbor-repository')
//                 IMAGE_PUSH_DESTINATION="${HARBOR_URL}/${HARBOR_REPOSITORY}/camp-go-example"
//                 GIT_COMMIT="${checkout (scm).GIT_COMMIT}"
//                 IMAGE_TAG = "${BRANCH_NAME}-${GIT_COMMIT}"
//                 BUILD_IMAGE="${IMAGE_PUSH_DESTINATION}:${IMAGE_TAG}"
//                 BUILD_IMAGE_LATEST="${IMAGE_PUSH_DESTINATION}:latest"
//             }

//             steps {
//                 // script {
//                 //     properties([pipelineTriggers([pollSCM('* * * * *')])])
//                 // }
//                 container(name: 'kaniko', shell: '/busybox/sh') {
//                     withEnv(['PATH+EXTRA=/busybox']) {
//                         sh '''#!/busybox/sh
//                             /kaniko/executor --force --context `pwd` --insecure --skip-tls-verify --cache=true --destination $BUILD_IMAGE --destination $BUILD_IMAGE_LATEST
//                         '''
//                     }
//                 }
//             }
//         }

//         stage('Cosign Image') {
//             agent {
//                 kubernetes {
//                     defaultContainer 'cosign'
//                     yaml """
// kind: Pod
// spec:
//   containers:
//   - name: cosign
//     # can not use official image: gcr.io/projectsigstore/cosign here, don't know why
//     image: lyzhang1999/cosign:latest@sha256:8c09f25dc815584fa4840a4adb0263b96ed5d3275068e79e74c0f02e636d14dd
//     imagePullPolicy: Always
//     command:
//     - sleep
//     args:
//     - 99d
//     volumeMounts:
//       - name: jenkins-docker-cfg
//         mountPath: /root/.docker
//       - name: cosign-key
//         mountPath: /root
//   volumes:
//   - name: jenkins-docker-cfg
//     projected:
//       sources:
//       - secret:
//           name: regcred
//           items:
//             - key: .dockerconfigjson
//               path: config.json
//   - name: cosign-key
//     projected:
//       sources:
//       - secret:
//           name: cosign-key-file
//           items:
//             - key: cosign.key
//               path: cosign.key
// """
//                 }
//             }
            
//             environment {
//                 HARBOR_URL     = credentials('harbor-url')
//                 HARBOR_REPOSITORY     = credentials('harbor-repository')
//                 COSIGN_KEY_PASSWORD = credentials('cosign-key-password')
//                 IMAGE_PUSH_DESTINATION="${HARBOR_URL}/${HARBOR_REPOSITORY}/camp-go-example"
//                 GIT_COMMIT="${checkout (scm).GIT_COMMIT}"
//                 IMAGE_TAG = "${BRANCH_NAME}-${GIT_COMMIT}"
//                 BUILD_IMAGE="${IMAGE_PUSH_DESTINATION}:${IMAGE_TAG}"
//                 BUILD_IMAGE_LATEST="${IMAGE_PUSH_DESTINATION}:latest"
//             }

//             steps {
//                 container(name: 'cosign', shell: '/bin/sh') {
//                     sh '''#!/bin/sh
//                         COSIGN_PASSWORD=$COSIGN_KEY_PASSWORD cosign sign --key /root/cosign.key $BUILD_IMAGE -y
//                     '''
//                 }
//             }
//         }

        stage('Scan Image with Grype') {
            agent {
                kubernetes {
                    defaultContainer 'grype'
                    yaml """
kind: Pod
spec:
  containers:
  - name: grype
    image: lyzhang1999/grype:latest@sha256:ca4ff9625f632d3a5e686089f4fa88068f5aa5cc8fa7d49115397f035f04e65a
    imagePullPolicy: Always
    command:
    - sleep
    args:
    - 99d
    volumeMounts:
      - name: jenkins-docker-cfg
        mountPath: /root/.docker
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
                HARBOR_URL     = credentials('harbor-url')
                HARBOR_REPOSITORY     = credentials('harbor-repository')
                COSIGN_KEY_PASSWORD = credentials('cosign-key-password')
                IMAGE_PUSH_DESTINATION="${HARBOR_URL}/${HARBOR_REPOSITORY}/camp-go-example"
                GIT_COMMIT="${checkout (scm).GIT_COMMIT}"
                IMAGE_TAG = "${BRANCH_NAME}-${GIT_COMMIT}"
                BUILD_IMAGE="${IMAGE_PUSH_DESTINATION}:${IMAGE_TAG}"
                BUILD_IMAGE_LATEST="${IMAGE_PUSH_DESTINATION}:latest"
            }

            steps {
                container(name: 'grype', shell: '/bin/sh') {
                    sh '''#!/bin/sh
                        grype $BUILD_IMAGE_LATEST --scope all-layers --fail-on high
                    '''
                }
            }
        }

    }
}
