pipeline{
    agent any
    environment{
        EMAIL_BODY =
            """
                <p>EXECUTED: Job <strong>\'${env.JOB_NAME}:${env.BUILD_NUMBER}\'</strong></p>
                <p>
                View console output at 
                "<a href="${env.BUILD_URL}">${env.JOB_NAME}:${env.BUILD_NUMBER}</a>"
                </p> 
                <p><em>(Build log is attached.)</em></p>

            """
            EMAIL_SUBJECT_SUCCESS = "Status: 'SUCCESS' -Job \'${env.JOB_NAME}:${env.BUILD_NUMBER}\'" 
            EMAIL_SUBJECT_FAILURE = "Status: 'FAILURE' -Job \'${env.JOB_NAME}:${env.BUILD_NUMBER}\'"
            EMAIL_SUBJECT_TEST_FAILURE = "Status: 'TEST FAILURE' -Job \'${env.JOB_NAME}:${env.BUILD_NUMBER}\'"
            EMAIL_RECEPIENT = 'dianaakara@gmail.com' 
    }
    tools{
        nodejs 'Nodejs'
    }
    stages{
        stage ('clone repository'){
           steps{
                git 'https://github.com/Akarad/gallery'
           }
        }
        stage ('Tests stage'){
           steps{
                sh 'npm test'
           }
            post{
                success{
                    emailext attachLog: true,
                    body: EMAIL_BODY,
                    subject: EMAIL_SUBJECT_TEST_FAILURE,
                    to: EMAIL_RECEPIENT  
                }
            }
        }
        stage ('Build stage'){
           steps{
                sh 'npm install'
           }
        }
        stage ('Deploy to heroku'){
           steps{
                withCredentials([usernameColonPassword(credentialsId: 'heroku', variable: 'HEROKU_CREDENTIALS')]){
                    sh 'git push https://${HEROKU_CREDENTIALS}@git.heroku.com/moringa-dark-room.git master'
                }
            }
        }
    }    
}