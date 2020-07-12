pipeline {
  agent any
  stages {
    stage('Initialize') {
      steps {
        echo 'Starting the pipeline'
        sh 'mvn clean'
      }
    }
    stage('Build') {
      steps {
        sh 'mvn -Dmaven.test.failure.ignore=true install'
      }
    }
    stage('Test') {
      steps {
        sh 'mvn -Dtest=AlexaControllerTest test'
      }
    }
    stage('Deploy') {
      steps {
        // Example AWS credentials
        withCredentials(
                [[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    credentialsId: '3709303f-bc22-4d32-80b2-cee096454e88',  // ID of credentials in Jenkins
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                ]])
                {
                    echo "Listing contents of an S3 bucket."
                    sh '''
                        export AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID}
                        export AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY}
                        export AWS_REGION=us-east-1
                        aws s3 ls
                        
                  
                  '''
                }
        archiveArtifacts 'target/*.war'
        //sh '''export AWS_ACCESS_KEY_ID=\${AWS_ID}'''
        //sh '''export AWS_SECRET_ACCESS_KEY=\${AWS_SECRET}'''
        //sh '''aws --debug s3 cp /var/lib/jenkins/workspace/alexa-cicd/target/alexa-cicd-0.0.1-SNAPSHOT.war s3://elasticbeanstalk-us-east-1-000902953924/2018362ew4-alexa-cicd-0.0.1-SNAPSHOT.war '''
       // sh 'aws --debug elasticbeanstalk create-application-version --application-name alexacicd --version-label "alexacicd-jenkins$BUILD_DISPLAY_NAME" --description "Created by $BUILD_TAG"  --source-bundle=S3Bucket=elasticbeanstalk-us-east-1-000902953924,S3Key=2018362ew4-alexa-cicd-0.0.1-SNAPSHOT.war'
       // sh 'aws elasticbeanstalk update-environment --environment-name=Alexacicd-env-1 --version-label "alexacicd-jenkins$BUILD_DISPLAY_NAME"'
      }
    }
  }
}
