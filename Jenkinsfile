pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        S3_BUCKET = 'freestyle-frontend'
        DISTRIBUTION_ID = 'E2DTAK5PHKKMJ6'
    }

    stages {

        stage('Deploy to S3') {
            steps {
                withAWS(credentials: 'aws-jenkins', region: 'us-east-1') {

                    sh '''
                    aws s3 sync . s3://$S3_BUCKET --delete \
                    --exclude ".git/*" \
                    --exclude "Jenkinsfile" \
                    --exclude "README.md"
                    '''
                }
            }
        }

        stage('Invalidate CloudFront Cache') {
            steps {
                withAWS(credentials: 'aws-jenkins', region: 'us-east-1') {

                    sh '''
                    aws cloudfront create-invalidation \
                    --distribution-id $DISTRIBUTION_ID \
                    --paths "/*"
                    '''
                }
            }
        }
    }
}