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
        nexusUrl = 'nexus.muvva.online:8081'
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
         stage('building artifact'){
            steps{
                sh """
                   zip -q -r backend-${appVersion}.zip * -x Jenkinsfile -x backend-${appVersion}.zip
                   ls -ltr
                """
            }
        }
        //  stage('sonar scaanning'){
        //     environment{
        //         scannerHome = tool 'sonar' //referring sonar scanner
        //     }
        //     steps{
        //         script{
        //             withSonarQubeEnv('sonar'){ //referring sonarqube server
        //                 sh " ${scannerHome}/bin/sonar-scanner"
        //             }
        //         }
        //     }
        // }
        stage('uploading artifact'){
            steps{
               script{
                nexusArtifactUploader(
                nexusVersion: 'nexus3',
                protocol: 'http',
                nexusUrl: "${nexusUrl}",
                groupId: 'com.expense',
                version: "${appVersion}",
                repository: "backend",
                credentialsId: 'nexus-auth',
                artifacts: [
                    [artifactId: "backend",
                    classifier: '',
                    file: 'backend-' + "${appVersion}" + '.zip',
                    type: 'zip']
                ]
            )
               }
            }
        }
        stage('Deploy')
        {
            steps{
                script{
                    def params = [
                        string(name: 'appVersion', value: "${appVersion}")
                    ]
                    build job: "backend-deploy",
                    parameters: params , wait: false
                }
            }
        }
     }
        post{
            always{
                echo "Running always"
                deleteDir()
            }
        }
    }