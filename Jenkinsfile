pipeline {
    agent any

    environment {
        UMAI_API_KEY = credentials('umai-api-key')
        PATH = "/usr/local/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Run UMAI Tests') {
            steps {
                sh '''
                    mkdir -p umai_data/pipeline_inputs
                    mkdir -p umai_data/outputs

                    echo "$UMAI_API_KEY" > umai_data/pipeline_inputs/api_key.txt

                    docker pull --platform linux/amd64 usemangoai/test-runner:latest

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
            junit allowEmptyResults: true, testResults: 'umai_data/outputs/junit.xml'
        }
    }
}
