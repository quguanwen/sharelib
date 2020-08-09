#！groovy

@Library('sharelib')

def tools = new org.devops.tools()

pipeline{

    agent{
        node{
          label "base-jnlp"  //指定对应标签的构建节点
          customWorkspace "/home"  //指定运行的工作目录（可选${workspace}）

        }
    }

    options{
        timestamps()  //记录该范围内所有阶段的日志
        skipDefaultCheckout()  //删除隐式的checkout scm语句，pipline会默认去检测拉代码
        disableConcurrentBuilds()  //禁止并行执行阶段
        timeout(time: 1, unit: 'HOURS')  //流水线整体超时设置1h

    }

    stages{
        stage("clone"){
            steps{  //步骤
                timeout(time: 5, unit: 'MINUTES'){ //步骤超时时间
                script{  //脚本语法
                    println('拉代码')
                    }
                }
            }
        }

        stage("sonar"){
            steps{
                timeout(time: 10, unit: 'MINUTES'){//超时时间
                script{
                     tools.PrintMes("sharelib")
                    }
                }
            }
        }

        stage("build"){
            steps{
                timeout(time: 20, unit: 'MINUTES'){ //构建超时时间
                echo "应用构建"
                }
            }
        }

        stage("build-docker"){
            steps{
                timeout(time: 10, unit: 'MINUTES'){//镜像构建时间
                echo "镜像构建，上传仓库"
                }
            }
        }

        stage("deploy"){
            steps{
                timeout(time: 10, unit: 'MINUTES'){ //部署到集群
                echo "发布应用"
                }
            }
        }

    }

    post{
        always{
            script{
                println("always")
            }
        }

        success{
            script{
                //发送通知等
                currentBuild.description = "构建成功"  //这里指的是左侧构建情况描述
            }
        }

        failure{
            script{
                //发送通知等
                currentBuild.description = "构建失败"  //这里指的是左侧构建情况描述
            }
        }

        aborted{
            script{
                //发送通知等
                currentBuild.description = "构建取消"  //这里指的是左侧构建情况描述
            }
        }
    }
}