pipeline {
    agent any
    tools{
        maven "maven3.8.5"
    }
    options {
  buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '3')
  timestamps ()
}

    triggers {
  pollSCM '20 15 5 * *'
}

    
    stages{
        stage('code checkout'){
            steps{
                git branch: 'J2EE', url: 'https://github.com/DevOpsPractice247/onlinebookstore.git'
            }
        }
        stage('code build'){
            steps{
                bat "mvn package"
            }
        }
        stage('store artfifactory'){
            steps{
                bat "mvn install"
            }
        }
    }
    post{
        always{
            slackSend channel: 'general', color: 'warning', message: 'BUILD COMPLETED'
            emailext attachLog: true, body: '''Report from project - ${PROJECT_NAME},
            Build number #${BUILD_NUMBER} 
            ${CAUSE}
Build status - ${BUILD_STATUS}
url - ${BUILD_URL}
${CHANGES}''', subject: '${DEFAULT_SUBJECT}', to: 'pujarianveshkumar@gmail.com,poojarianveshkumar@outlook.com'
        }
        success{
            slackSend channel: 'general', color: 'good', message: 'Build SUCCESSFULL'
        }
        failure{
            slackSend channel: 'general', color: 'danger', message: 'BUILD FAILED'
        }
    }
}
