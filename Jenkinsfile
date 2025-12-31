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
        appVersion= ""
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
                script{
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
            script{
                    def packageJSON = readJSON file: 'package.json'
                    appVersion = packageJSON.version
                    echo "app verison: ${appVersion}"
                }
            }
        }

                stage('Install Deps') {
            steps {
                script{
                    sh """
                            npm install


                       """ 
                }
            }
        }
        stage('Deploy') {

            when {
                // ver important for deployment
                expression {
                     "${params.DEPLOY}" == "true" 
                }
                
                }

            steps {
                script{
                    sh """
                        echo "Building----------A-Deployment------------------WHEN-CONDITION"
                       """ 
                }
            }
        }
    }

    // --------------------------post build--------------------
        post { 
        always { 
            echo 'I will always say Hello again!'
            cleanWs()
        }
        success{
            echo "Success---------------"
        }
        failure{
            echo "failure---------------"
        }
        aborted{
            echo "someone -aborted--------------------------------"
            echo "may be timeeout  -aborted--------------------------------"
        }
    }
}