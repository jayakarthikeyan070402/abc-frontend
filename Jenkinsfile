pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'ca-central-1'
        S3_BUCKET = 'karthik-frontend-2026 '
        CLOUDFRONT_DISTRIBUTION_ID = 'E204QJRLTLZ5GI'
    }

    stages {
        stage ('clone frontend repo') {
            steps {
              git 'https://github.com/jayakarthikeyan070402/abc-frontend.git',branch:'main',credentialsId:'jayakarthikeyan070402'
            }
        }
        stage ('install & build') {
            steps {
                sh '''
                npm install
                npm run build
                '''
            }
        }
        stage ('deploy to s3') {
            steps {
                withAWS(credentials: 'aws-task-cred',region: 'ca-central-1') {
                    sh '''
                    echo "uploading to s3"
                    aws s3 sync build/ s3://$S3_BUCKET --delete
                    
                    echo "invalidating cloudfront cache"
                    aws cloudfront create-invalidation --distribution-id $CLOUDFRONT_DISTRiBUTION_ID --paths "/*"
                    '''
                }
            }
        }
    }
}
