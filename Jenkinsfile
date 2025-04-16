pipeline {
    agent any
    stages {
        stage('Build & Test') {
            steps {
                script {
                    // 判断是否是 PR 触发
                    if (env.CHANGE_ID) {
                        echo "Building Pull Request #${env.CHANGE_ID}"
                    } else {
                        echo "Building Branch: ${env.BRANCH_NAME}"
                    }
                }
                // 执行构建命令（例如 Maven）
                sh 'echo "mvn clean package"'
            }
        }
        stage('Report Status') {
            steps {
                // 将构建状态回传到 GitHub PR
                script {
                    if (env.CHANGE_ID) {
                        githubNotify status: currentBuild.currentResult, 
                                    context: 'Jenkins/PR-Build', 
                                    description: "Build ${currentBuild.currentResult}"
                    }
                }
            }
        }
    }
}
