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
        stage('read the version'){
            steps{
                script{
                    def packageJSON = readJSON file: 'package.json'
                    def appVersion = packageJSON.version
                    echo "app version is: $appVersion"
                }
            }
        }
        
        stage('installing dependencies'){
            steps{
                sh """
                   npm install
                   ls -ltr
                    echo "app version is: $appVersion"
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