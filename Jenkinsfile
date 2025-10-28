pipeline {
    agent any
    environment {
        DISABLE_AUTH = 'true'
        DB_ENGINE    = 'sqlite'
    }
    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: '选择分支')
        choice(name: 'ENV', choices: ['dev', 'staging', 'prod'], description: '部署环境')
    }
    options {
        timestamps()
    }
    stages {
        stage('Build') {
            steps {
                sh 'echo "Hello World"'
                sh '''
                    echo "Multiline shell steps works too"
                    ls -lah
                '''
                sh 'printenv'
            }
        }
        stage('Test') {
            agent {
                docker { image 'node:7-alpine' }
            }
            when {
                expression {params.ENV == "dev" || params.ENV == "prod"}
            }
            steps {
                sh 'node --version'
            }
        }
        stage('Deploy') {
            when {
                expression {params.ENV == "prod"}
            }
            steps {
                sh 'sh deploy.sh'
            }
        }
        stage('Check') {
            when {
                expression {params.ENV == "prod"}
            }
            steps {
                dir('/opt') {
                    sh 'ls -lah'
                }
            }
        }
    }
    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}