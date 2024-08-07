pipeline{
    agent {
        label 'AGENT-1'
    }
    options{
        timeout(time: 30,unit: 'MINUTES')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    stages{
        stage('test'){
            steps{
                sh """
                   echo "This is testing for backend pipeline"
                """
            }

        }
     }
        post{
            always{
                deleteDir()
            }
        }
    }