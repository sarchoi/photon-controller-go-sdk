pipeline {
    agent any
    environment {
        DEFAULT_AWS_REGION = 'us-west-2'        
    }
    stages {
        stage('Build') {
            steps {
                echo "Building ${env.JOB_NAME}:${env.BUILD_ID} on ${env.JENKINS_URL}.."
                sh """
                    echo "BUILD stage"
                """
            }
        }
        stage('Test') {
            steps {
                awsIdentity()
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-jenkins', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                    sh """
                        echo "TEST stage"
                        date >> date.txt
                        cat date.txt
                        curl https://s3-us-west-2.amazonaws.com/gophoton-stress/hello.txt
                        aws s3 cp s3://gophoton-stress/hello.txt .
                        cat hello.txt
                        aws s3 cp date.txt s3://gophoton-stress/test/date.txt                        
                    """
                }
            }
        }
        stage('Publish') {
            when {
                // Only publish master branch
                expression { env.JOB_BASE_NAME == 'master' }
            }
            steps {
                echo "Publishing ${env.JOB_NAME}:${env.BUILD_ID} on ${env.JENKINS_URL}.."
                sh """
                    echo "PUBLISH stage"
                """
            }
        }
        stage('Deploy') {
            when {
                // Only deploy master branch
                expression { env.JOB_BASE_NAME == 'master' }
            }
            steps {
                echo 'Deploying ${env.JOB_NAME}:${env.BUILD_ID} on ${env.JENKINS_URL}..'
                sh """
                    echo "DEPLOY stage"
                """
            }
        }
    }
}
