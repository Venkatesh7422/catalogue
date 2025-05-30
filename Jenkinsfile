pipeline {
    agent {
        node {
            label 'agent-1'
    }
    }
    environment {
            packageVersion = '' 
            nexusURL = '172.31.84.49:8081'
         }
    options {
        timeout(time: 1, unit: 'HOURS')
        disableConcurrentBuilds()
    }
    // parameters {
    //     string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')

    //     text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')

        booleanParam(name: 'Deploy', defaultValue: false, description: 'Toggle this value')

    //     choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')

    //     password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    // }
    //build
    stages {
        stage('Get the Version') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    packageVersion = packageJson.version
                    echo "application version: $packageVersion"
                }
            }
        }
        stage('install dependencies') {
            steps {
                sh """
                npm install
                """
            }
        }
        stage('Unit tests') {
            steps {
                sh """
                echo "unit tests will run here"
                """
            }
        }
        stage('Sonar Scan') {
            steps {
                sh """
                  sonar-scanner
                """
            }
        }
        stage('Build') {
            steps {
                sh """
                    ls -la
                    zip -q -r catalogue.zip ./* -x ".git" -x "*.zip"
                    ls -ltr
                """
            }
        }
         stage('Publish Artifact') {
            steps {
                 nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: "${nexusURL}",
                    groupId: 'com.roboshop',
                    version: "${packageVersion}",
                    repository: 'catalogue',
                    credentialsId: 'nexus-auth',
                    artifacts: [
                        [artifactId: 'catalogue',
                        classifier: '',
                        file: 'catalogue.zip',
                        type: 'zip']
                    ]
                )
            }
        }
        stage('Deploy') {
            when {
                expression{
                params.Deploy  == true
                }
            }
            steps {
                 script {
                        def params = [
                            string(name: 'version', value: "${packageVersion}"),
                            string(name: 'environment', value: "dev")
                        ]
                         build job: "catalogue-deploy", wait: true, parameters: params
                    }
            }
        }    
    } 
    //psot build
      post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir()        
        }
        failure{
            echo 'this runs when pipeline is failed, used generally to send some alerts'
        }
        success{
            echo 'I will say Hello, when pipeline is success'
        }
    }
}
}



// ************************************************************************************************************************
// chatgpt adjusted version upto 44 session
// pipeline {
//     agent {
//         node {
//             label 'agent-1'
//         }
//     }
//     environment {
//         packageVersion = '' 
//         nexusURL = '13.219.80.75:8081'
//     }
//     options {
//         timeout(time: 1, unit: 'HOURS')
//         disableConcurrentBuilds()
//     }
//     stages {
//         stage('Get the Version') {
//             steps {
//                 script {
//                     def packageJson = readJSON file: 'package.json'
//                     packageVersion = packageJson.version
//                     echo "application version: $packageVersion"
//                 }
//             }
//         }
//         stage('install dependencies') {
//             steps {
//                 sh "npm install"
//             }
//         }
//         stage('Build') {
//             steps {
//                 sh """
//                     ls -la
//                     zip -q -r catalogue.zip ./* -x ".git" -x "*.zip"
//                     ls -ltr
//                 """
//             }
//         }
//         stage('Publish Artifact') {
//             steps {
//                 nexusArtifactUploader(
//                     nexusVersion: 'nexus3',
//                     protocol: 'http',
//                     nexusUrl: "${nexusURL}",
//                     groupId: 'com.roboshop',
//                     version: "${packageVersion}",
//                     repository: 'catalogue',
//                     credentialsId: 'nexus-auth',
//                     artifacts: [
//                         [artifactId: 'catalogue',
//                          classifier: '',
//                          file: 'catalogue.zip',
//                          type: 'zip']
//                     ]
//                 )
//             }
//         }
//         stage('Deploy') {
//             steps {
//                 script {
//                     build job: "catalogue-deploy",
//                         wait: true,
//                         parameters: [
//                             string(name: 'version', value: "${packageVersion}"),
//                             string(name: 'environment', value: "dev")
//                         ]
//                 }
//             }
//         }
//     }
//     post {
//         always {
//             echo 'I will always say Hello again!'
//             deleteDir()
//         }
//         failure {
//             echo 'this runs when pipeline is failed, used generally to send some alerts'
//         }
//         success {
//             echo 'I will say Hello, when pipeline is success'
//         }
//     }
// }
