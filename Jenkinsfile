pipeline {
    agent { label 'build_java_11' }
    options { 
        timeout(time: 1, unit: 'HOURS')
        retry(2) 
    }
    triggers {
        cron('0 * * * *')
    }
    parameters {
        choice(name: 'GOAL', choices: ['compile', 'package', 'clean package'])
    }
    stages {
        stage('Source Code') {
            steps {
                git url: 'https://github.com/munnakona/spring-petclinic.git', 
                branch: 'main'
            }

        }
        stage('Build the Code') {
            steps {
                sh script: "mvn ${params.GOAL}"
                stash name: 'spc-build-jar', includes: 'target/*.jar'
            }
        }
        stage('reporting') {
            steps {
                junit testResults: 'target/surefire-reports/*.xml'
            }  
        }
        stage('deployment') {
            steps {
                unstash name: 'spc-build-jar'
                echo "clone the latest playbook"
                sh 'ansible --version'
            }
        }
    }
    post {
        success {
            // send the success email
            echo "Success"
        }
        unsuccessful {
            //send the unsuccess email
            echo "failure"
        }
    }
}
