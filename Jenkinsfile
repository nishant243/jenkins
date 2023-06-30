pipeline {
    agent any

    environment {
        function_name = 'Jenkinsjava'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Build'
                sh 'mvn package'
            }
        }
        stage("sonarqube analysis") {
              agent any
              steps {
                  withSonarQubeEnv('sonar') {
                      sh 'mvn clean package sonar:sonar'
                  }
              }
         }

        stage('Push') {
            steps {
                echo 'Push'

                sh "aws s3 cp target/sample-1.0.3.jar s3://jenkins121"
            }
        }

        stage('Deploy') {
            steps {
                echo 'Build'

                sh "aws lambda update-function-code --function-name $function_name --region us-east-2 --s3-bucket jenkins121 --s3-key sample-1.0.3.jar"
            }
        }
    }
}
