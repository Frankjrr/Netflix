pipeline {
    agent any
    tools{
        jdk 'jdk17'
        nodejs 'node16'
    }
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }
    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
        stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Netflix \
                    -Dsonar.projectKey=Netflix '''
                }
            }
        }
        stage("quality gate"){
           steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonarqube-token' 
                }
            } 
        }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        // stage('Build Image') {
        //     steps {
        //         script {
        //             echo "Building Docker image"
        //             withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
        //                 sh """
        //                 docker build -t hassantariq14351/demo-app:Netflix-1.0 .
        //                 echo \$PASS | docker login -u \$USER --password-stdin
        //                 docker push hassantariq14351/demo-app:Netflix-1.0
        //                 """
        //             }
        //         }
        //     }
        // }
        stage('Deploy') {
            steps {
                script {
                    echo "Deploying"
                    // Add your deployment steps here
                }
            }
        }
    }
}
