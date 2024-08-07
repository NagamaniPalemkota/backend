pipeline{
    agent {
        label 'AGENT-1'
    }
    options{
        timeout(time: 30,unit: 'MINUTES')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    environment{
        def appVersion = ''
    }
    stages{
        stage('read the version'){
            steps{
                script{
                    def packageJSON = readJSON file: 'package.json'
                    appVersion = packageJSON.version
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