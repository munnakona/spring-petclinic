pipeline{
    agent{label 'build_java_11'}
    options{
        timeout(time: 1, unit: 'HOURS')
        retry(2)
    }
    triggers{
        cron('0 * * * *')
    }
    stages{
        stage('sourcecode'){
            steps{
                git url: 'https://github.com/munnakona/spring-petclinic.git'
                branch:'main'
            }
            stage('Build the code'){
              steps{
                   sh script:'mvn clean package'
              } 
            }
            stage('reporting'){
                steps{
                     junit testResults: 'target/surefire-reports/*.xml'
                } 

            }
        }

    }
        posts{
            success{
                //send the success email
                echo "Success"
            }
            unsuccess{
                //send the unsucess email
                echo " Failure"
            }

        }
}

