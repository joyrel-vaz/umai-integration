pipeline {
    agent any

    environment {
        UMAI_API_KEY = credentials('umai-api-key')
        PATH = "/usr/local/bin:${env.PATH}"

    }

    stages {
        stage('Run UMAI Tests') {
            steps {
                sh '''
                    docker pull --platform linux/amd64 usemangoai/test-runner:latest

                    echo "$UMAI_API_KEY" > umai_data/pipeline_inputs/api_key.txt
                    mkdir -p umai_data/outputs

                    docker run --rm \
                      --platform linux/amd64 \
                      -v ${WORKSPACE}/umai_data:/umai_data \
                      -e GENERATE_JUNIT_REPORT=true \
                      usemangoai/test-runner:latest
                '''
            }
        }
    }

    post {
        always {
            junit 'umai_data/outputs/junit.xml'
        }
    }
}