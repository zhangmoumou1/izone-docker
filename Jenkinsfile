pipeline {
    agent {
        label 'docker'
    }
    options {
        timestamps()
    }
    environment {
        GITHUB_USER_ID = '32025603'
        ALIYUN_USER_ID = '100006655230'
        IMAGE_NAME = 'ccr.ccs.tencentyun.com/zhangyancheng/public_izone:1.0'
    }
    stages {
        stage('Clone sources') {
            options {
                timeout(time: 30, unit: 'SECONDS')
            }
            steps {
                git (credentialsId: "${GITHUB_USER_ID}", url: 'https://github.com/zhangmoumou1/izone.git', branch: 'main')
            }
        }
        stage('Build image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}")
                }
            }
        }
        stage('Push image') {
            steps {
                withDockerRegistry(credentialsId: "${ALIYUN_USER_ID}", url: 'ccr.ccs.tencentyun.com/zhangyancheng/public_izone:1.0') {
                    sh "docker push ${IMAGE_NAME}"
                }
            }
        }
    }
    post {
        always {
            sh "docker images|grep '<none>'|awk '{print \$3}'|xargs docker image rm > /dev/null 2>&1 || true"
            sh "docker images"
            cleanWs()
        }
    }
}