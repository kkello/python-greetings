pipeline {
    agent any
    stages {
        stage('build-docker-image') {
            steps {
                build_docker_image()
            }
        }
        stage('deploy-to-dev') {
            steps {
                deploy_to_environment("dev")
            }
        }
        stage('tests-on-dev') {
            steps {
                start_api_tests("dev")
            }
        }
        stage('deploy-to-stg') {
            steps {
                deploy_to_environment("stg")
            }
        }
        stage('tests-on-stg') {
            steps {
                start_api_tests("stg")
            }
        }
        stage('deploy-to-prod') {
            steps {
                deploy_to_environment("prod")
            }
        }
        stage('tests-on-prod') {
            steps {
                start_api_tests("prod")
            }
        }
    }
}

def build_docker_image(){
    echo "Building docker image"
    sh "docker build --no-cache -t kkello/python-greetings-app:latest ."

    echo "Pushing docker image"
    sh "docker push kkello/python-greetings-app:latest"
}

def deploy_to_environment(String environment){
    echo "Deploying to: ${environment}"
    sh "docker pull kkello/python-greetings-app:latest"

    echo "Stopping and removing previous environment"
    sh "docker-compose stop greetings-app-${environment}"
    sh "docker-compose rm greetings-app-${environment}"

    echo "Starting docker again"
    sh "docker-compose up -d greetings-app-${environment}"
}

def start_api_tests(String environment){
    echo "Starting API tests"
    sh "docker pull kkello/api-tests:latest"

    echo "Executing API tests for ${environment}"
    sh "docker run --network=host --rm kkello/api-tests:latest run greetings greetings_${environment}"
}
