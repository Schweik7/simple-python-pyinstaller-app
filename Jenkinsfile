pipeline {
    agent none  //none参数意味着不会为 整个流水线的执行分配全局代理
    stages {
        stage('Build') { 
            agent {
                docker {
                    image 'python:2-alpine' //这个容器寿命较短 - 他的生命周期只是 你的 Build 阶段的执行的时间
                }
            }
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py' 
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'qnib/pytest'
                }
            }
            steps {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
    }
}