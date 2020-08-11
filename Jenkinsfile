pipeline {
     agent any
     stages {
         stage('Build') {
             when {
                 branch 'dev'
             }
             steps {
                 sh 'echo "Hello World"'
                 sh '''
                     echo "Multiline shell steps works too"
                     ls -lah
                 '''
             }
         }
         stage('Lint HTML') {
             when {
                 branch 'stage'
             }
              steps {
                  sh 'tidy -q -e *.html'
              }
         }
         stage('Security Scan') {
             when {
                 branch 'stage'
             }
              steps { 
                 aquaMicroscanner imageName: 'alpine:latest', notCompleted: 'exit 1', onDisallowed: 'fail'
              }
         }         
         stage('Upload to AWS') {
             when {
                 branch 'deploy'
             }
              steps {
                  withAWS(region:'us-west-2',credentials:'udacityDevops') {
                  sh 'echo "Uploading content with AWS creds"'
                      s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index.html', bucket:'static-jenkins-pipeline')
                  }
              }
         }
     }
}