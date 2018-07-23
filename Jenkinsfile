pipeline{
agent any
stages {
    stage('Checkout code') {
        steps {
            checkout scm
        }
    }
    stage('Create API') {
        steps {
            sh 'curl -X POST -d @input.schema http://mag.gateway.day.apim.ca.com:8080/pushToLac -H "Content-Type: application/json"'
        }
    }
   stage('Deploy API to Test') {
        steps {
            sh 'curl -X POST -d @swagger.json http://mag.gateway.day.apim.ca.com:8080/deployToPortal -H "Content-Type: application/json"'     
        }
    }
    stage('Build Tests') {
        steps {
            sh 'curl -X POST -d @swagger.json -H "Content-Type: application/json" http://mag.gateway.day.apim.ca.com:8080/buildBlazeTest > file.json'     
        }
    }
    stage('Run Unit Tests ') {
        steps {
           sh 'bzt file.json .bzt-rc'
        }
    }

   
}
}
