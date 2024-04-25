pipeline {
    agent {
        node {
            label 'AGENT-2'
        }
    }
    options {
        ansiColor('xterm')
        // timeout(time: 1, unit: 'HOURS')
        // disableConcurrentBuilds()
    }
    parameters {
        string(name: 'component', defaultValue: 'vpc', description: 'Which component to run?')
        choice(name: 'action', choices: ['apply', 'destroy'], description: 'Pick something')
    }
    // build
    stages {
        stage('Init') {
            steps {
                sh """
                    cd ${param.component}
                    terraform init -reconfigure
                """
            }
        }
        stage('Plan') {
            steps {
                sh """
                    cd ${param.component}
                    terraform plan
                """
            }
        }
        stage('Deploy') {
            when {
                expression { 
                    params.action == 'apply'
                }
            }
            input {
                message "Should we continue?"
                ok "Yes, we should."
                // submitter "alice,bob"
                // parameters {
                //     string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                // }
            }
            steps {
                sh """
                    cd ${param.component}
                    terraform apply -auto-approve
                """
            }
        }
        stage('Destroy') {
            when {
                expression { 
                    params.action == 'destroy'
                }
            }
            input {
                message "Should we continue?"
                ok "Yes, we should."
                // submitter "alice,bob"
                // parameters {
                //     string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                // }
            }
            steps {
                sh """
                    cd ${param.component}
                    terraform destroy -auto-approve
                """
            }
        }
    }
    // post build
    post { 
        always { 
            echo 'I will always say Hello again!'
        }
        failure { 
            echo 'this runs when pipeline is failed, used generally to send some alerts'
        }
        success{
            echo 'I will say Hello when pipeline is success'
        }
    }
}