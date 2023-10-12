pipeline {
    agent any
    tools{
        jdk 'jdk17' // jdk-17.0.8.1+1
        nodejs 'node16' // Nodejs 16.2.0
    }
    environment {
        SCANNER_HOME=tool 'sonar-scanner'  // sonnar sccaner plugin // version SonarQube Scanner 5.0.1.3006 
    }
    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout from Git'){
            steps{
                git branch: 'main', url: 'https://github.com/Frankjrr/Netflix.git'
            }
        }
        stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonar-server') { // global configuration name
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Netflix \
                    -Dsonar.projectKey=Netflix '''
                }
            }
        }
        stage("quality gate"){
           steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonarqube-token' // sonarqube secret token
                }
            } 
        }
        // stage('Install Dependencies') {
        //     steps {
        //         sh "npm install"
        //     }
        // }
        stage('Build Image') {
            steps {
                script {
                    echo "Building Docker image"
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh """
                        docker build --build-arg TMDB_V3_API_KEY=e5d8d5e6f6330a98515d0093e3d4b26f -t hassantariq14351/demo-app:netflix-1.0 .
                        echo \$PASS | docker login -u \$USER --password-stdin
                        docker push hassantariq14351/demo-app:netflix-1.0
                        """
                    }
                }
            }
        }
        stage('Deploy to container'){
            steps{
                sh 'docker run -d --name netflix -p 8081:80 hassantariq14351/demo-app:netflix-1.0'
            }
        }
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
