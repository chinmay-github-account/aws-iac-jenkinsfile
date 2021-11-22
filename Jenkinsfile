pipeline {
    agent any
	
	//tools {
        // Install the Maven version configured as "M3" and add it to the path.
       // maven "M3"
    //}

    stages {
        stage('Build') {
            steps {
		// Get some code from a GitHub repository
                // git 'https://github.com/chinmay-github-account/aws-iac-jenkinsfile.git'
				
		// Run Maven on a Unix agent.
                //sh "mvn -Dmaven.test.failure.ignore=true clean package"
				
                echo 'Building..'
		sh "aws cloudformation validate-template --template-body file://DynamoDB.yaml"
            }
        }
        stage('Test') {
            steps {
		
                echo 'Testing..'
		sh "aws cloudformation create-change-set --change-set-name $CloudFormation_ChangeSetName --change-set-type CREATE --stack-name $CloudFormation_StackName --region us-east-1 --template-body file://DynamoDB.yaml --tags Key=Application_Name,Value=$APP_NAME --role-arn $CloudFormation_Role --capabilities CAPABILITY_NAMED_IAM"
            }
        }
        stage('Deploy') {
            steps {
		
                echo 'Deploying....'
		sh "aws cloudformation execute-change-set --stack-name $CloudFormation_StackName --change-set-name $CloudFormation_ChangeSetName --region us-east-1"
		echo 'Verifying....'
		sh "aws cloudformation wait stack-create-complete --stack-name $CloudFormation_StackName --region us-east-1"
            }
        }
    }
}
