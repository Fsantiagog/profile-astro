pipeline{
    agent any
    environment {
        AWS_REGION = "us-east-1"
        BUCKET_NAME ="profile-website-fsantiago"
        DISTRIBUTION_ID="E2087UU0AMJNKK"
    }
    stages{
        stage('Checking code'){
            steps{
                git branch: 'master', credentialsId: '48b8e180-1b25-4ead-b1a4-b88cf8701745', url: 'git@github.com:Fsantiagog/profile-astro.git'
            }
        }

        stage('Deploying to S3 Bucket'){
            steps{
                script {
                    sh '''
                    rm -rf .git .github Jenkinsfile
                    aws s3 sync . s3://$BUCKET_NAME --delete --exact-timestamps
                    '''
                }
            }
        }
        stage('Clean up cache'){
            steps{
                script{
                    sh '''
                    echo "Invalidando caché de CloudFront..."
                        aws cloudfront create-invalidation --distribution-id $DISTRIBUTION_ID --paths "/
                    '''
                }
            }
        }
    }
}