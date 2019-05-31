pipeline{
agent any
stages {
    stage('Checkout code') {
        steps {
            checkout scm
        }
    }
    stage('Build API Server') {
        steps {
            sh 'docker-compose -f docker-compose-mysql-4_1.yml up -d'
            timeout(5) {
               waitUntil {
                  script {
                     def r = sh script: 'wget -q http://localhost:8081/APICreator/#/ -O /dev/null', returnStatus: true
                     return (r == 0);
                  }
               }
           }
        }
    }
    stage('Create API') {
        steps {
            sh 'curl -X POST -d @input.schema http://gw1.day.apim.ca.com:8080/pushToLac -H "Content-Type: application/json"'
            sleep 5
        }
    }
   stage('Deploy API to Test') {
        steps {
            sh 'curl -X POST -d @swagger.json http://gw1.day.apim.ca.com:8080/deployToPortal -H "Content-Type: application/json"'     
        }
    }
    stage('Build Tests') {
        steps {
            sh 'curl -X POST -d @swagger.json -H "Content-Type: application/json" http://gw1.day.apim.ca.com:8080/buildBlazeTest > file.json'     
        }
    }
    stage('Run Unit Tests ') {
        steps {
           sh 'bzt file.json .bzt-rc'
        }
    }

   
}
}
