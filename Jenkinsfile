pipeline {
    agent any
    environment {
        AWS_REGION = "${params.REGION}" // Set AWS region parameter
    }
    parameters {
        string(name: 'REGION', defaultValue: 'us-east-1', description: 'AWS region to list resources from')
    }
    stages {
        stage('Initialize') {
            steps {
                echo "Listing resources in AWS region: ${AWS_REGION}"
            }
        }
        stage('List Instances') {
            steps {
                script {
                    sh """
                    echo "Instances in region ${AWS_REGION}:"
                    aws ec2 describe-instances --region ${AWS_REGION} --query 'Reservations[*].Instances[*].{ID:InstanceId,Type:InstanceType,State:State.Name}' --output table
                    """
                }
            }
        }
        stage('List Volumes') {
            steps {
                script {
                    sh """
                    echo "Volumes in region ${AWS_REGION}:"
                    aws ec2 describe-volumes --region ${AWS_REGION} --query 'Volumes[*].{ID:VolumeId,Size:Size,State:State}' --output table
                    """
                }
            }
        }
        stage('List Security Groups') {
            steps {
                script {
                    sh """
                    echo "Security Groups in region ${AWS_REGION}:"
                    aws ec2 describe-security-groups --region ${AWS_REGION} --query 'SecurityGroups[*].{ID:GroupId,Name:GroupName}' --output table
                    """
                }
            }
        }
        stage('List Elastic IPs') {
            steps {
                script {
                    sh """
                    echo "Elastic IPs in region ${AWS_REGION}:"
                    aws ec2 describe-addresses --region ${AWS_REGION} --query 'Addresses[*].{IP:PublicIp,InstanceId:InstanceId}' --output table
                    """
                }
            }
        }
    }
}
