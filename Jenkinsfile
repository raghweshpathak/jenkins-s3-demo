pipeline {
    agent any

    environment {
        // Change if your bucket or region are different
        S3_BUCKET = 'jenkins-demo-bucket-raghwesh'
        AWS_REGION = 'ap-south-1'
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out source code from GitHub..."
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Running build step (demo)..."
                sh '''
                    echo "This file was created by Jenkins Pipeline build on $(date)" > build-artifact.txt
                    echo "Git commit: $(git rev-parse --short HEAD)" >> build-artifact.txt
                    echo "Repository: jenkins-s3-demo" >> build-artifact.txt
                    cat build-artifact.txt
                '''
            }
        }

        stage('Upload to S3') {
            steps {
                echo "Uploading build artifact to S3..."
                sh '''
                    aws s3 cp build-artifact.txt \
                      s3://'"$S3_BUCKET"'/pipeline-builds/build-$(date +%Y%m%d%H%M%S).txt \
                      --region '"$AWS_REGION"'
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline finished successfully ✅"
        }
        failure {
            echo "Pipeline failed ❌ – check the logs."
        }
    }
}
