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
        stage('installing dependencies'){
            steps{
                sh """
                   npm install
                   
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