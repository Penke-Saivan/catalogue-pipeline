pipeline {
    // ------------pre-build-----------------

    // agent configured
    agent {
        node {
            label 'AGENT-1'
        }
    }

    // environment variables
    environment {
        COURSE = 'Jenkin'
        appVersion = ''
        ACC_ID = '131676642204'
        PROJECT = 'roboshop'
        COMPONENT = 'catalogue'
    }
    //  after the timeout abort the pipeline
    options {
        timeout(time: 10, unit: 'MINUTES')
        disableConcurrentBuilds()
    }

    // ------------------- build stage -----------------------
    stages {
        stage('BUILD') {
            steps {
                script {
                    sh """
                        echo "Building----------A-Read-Version"
                        echo "COurse we learn is : $COURSE"

                       """
                }
            }
        }
        stage('Read-Version') {
            // input {
            //     message "Should we continue?"
            //     ok "Yes, we should."
            //     submitter "alice,bob"
            //     parameters {
            //         string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
            //     }}

            steps {
                script {
                    def packageJSON = readJSON file: 'package.json'
                    appVersion = packageJSON.version
                    echo "app verison: ${appVersion}"
                }
            }
            }

                stage('Install Deps') {
            steps {
                script {
                    sh '''
                            npm install

                       '''
                }
            }
                }

        // stage('Unit Test') {
        //     steps {
        //         script {
        //             sh '''
        //                 npm test
        //             '''
        //         }
        //     }
        // }
        //Here you need to select scanner tool and send the analysis to server
        stage('Sonar Scan') {
            environment {
                def scannerHome = tool 'sonar-8.0'
            }
            steps {
                script {
                    withSonarQubeEnv('sonar-server') {
                        sh  "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
        // stage('Quality Gate') {
        //     steps {
        //         timeout(time: 1, unit: 'HOURS') {
        //             // Wait for the quality gate status
        //             // abortPipeline: true will fail the Jenkins job if the quality gate is 'FAILED'
        //             waitForQualityGate abortPipeline: true
        //         }
        //     }
        // }

        // Build images
        stage('Build Images') {
            steps {
                script {
                    withAWS(region:'us-east-1', credentials:'aws-creds') {
    // do something-withAWS keeps aws creds ready for the code in this block

                        sh """
    echo "Building Image and pushing to ECR"
    aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
    docker build -t ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion} .
    docker images
    docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}

       """
                    }
                }
            }
        }
        //                 stage('Build Images') {
        //     steps {
        //         script{
        //             sh """
        //                     docker build -t catalogue:${appVersion} .
        //                     docker images

        //                """
        //         }
        //     }
        // }
        // stage('Deploy') {

        //     when {
        //         // ver important for deployment
        //         expression {
        //              "${params.DEPLOY}" == "true"
        //         }

        //         }

        //     steps {
        //         script{
        //             sh """
        //                 echo "Building----------A-Deployment------------------WHEN-CONDITION"
        //                """
        //         }
        //     }
        // }
        }

        // --------------------------post build--------------------
        post {
        always {
            echo 'I will always say Hello again!'
            cleanWs()
        }
        success {
            echo 'Success---------------'
        }
        failure {
            echo 'failure---------------'
        }
        aborted {
            echo 'someone -aborted--------------------------------'
            echo 'may be timeeout  -aborted--------------------------------'
        }
        }
    }
