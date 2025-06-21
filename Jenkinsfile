pipeline {
    agent any
    tools{
        maven 'maven'
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('deploy to app_server') {
            steps {
                sh '''
                    echo "Checking if Spring application is running..."
                    if pgrep -f target/spring_app_sak-0.0.1-SNAPSHOT.jar > /dev/null; then
                        echo "App is running. Stopping it..."
                        sudo pkill -f target/spring_app_sak-0.0.1-SNAPSHOT.jar
                        sleep 3
                    else
                        echo "App is not running. Skipping stop."
                    fi

                    echo "Starting the Spring application..."
                    sudo java -jar target/spring_app_sak-0.0.1-SNAPSHOT.jar > /dev/null 2>&1 &
                '''

            }
        }
        stage('deploy to nexus'){
            steps{
                sh 'mvn -s settings.xml deploy'
            }
            post {
                success {
                    echo "Deployed successfully to nexus"
                }
                failure {
                    echo "Failed to Deploy to nexus"
                }
            }
        }
    }
    post {
        success {
            echo "Deployed successfully"
        }
        failure {
            echo "Failed to Deploy"
        }
    }
}