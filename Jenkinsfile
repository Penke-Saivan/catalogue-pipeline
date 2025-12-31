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
    }
//  after the timeout abort the pipeline    
    options {
        timeout(time: 10, unit: 'MINUTES') 
        disableConcurrentBuilds()
    }

// ------------------- build stage -----------------------  
    stages {
        stage('Building') {
            steps {
                script{
                    sh """
                        echo "Building----------A-Build"
                        echo "COurse we learn is : $COURSE"

                       """ 
                }
            }
        }
        stage('Test') {
            
            // input {
            //     message "Should we continue?"
            //     ok "Yes, we should."
            //     submitter "alice,bob"
            //     parameters {
            //         string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
            //     }}

            steps {
            script{
                    sh """
                        echo "Building----------A-Test"
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