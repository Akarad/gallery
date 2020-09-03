pipeline{
    agent any
    tools{
        nodejs 'Nodejs'
    }
    stages{
        stage ('clone repository'){
           steps{
                git 'https://github.com/Akarad/gallery'
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