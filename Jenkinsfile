pipeline {
    agent any
    stages {
        stage('clean workspace'){
            steps{
                cleanWs()
            }
        }
        // stage("Sonarqube Analysis "){
        //     steps{
        //         withSonarQubeEnv(credentialsId: 'sonar-token') {
        //             sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Netflix \
        //             -Dsonar.projectKey=Netflix '''
        //         }
        //     }
        // }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        stage("build image") {
                steps {
                    script {
                    echo "building image"
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh 'docker build -t  hassantariq14351/demo-app:Netflix-1.0 .'
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push  hassantariq14351/demo-app:Netflix-1.0'
                        }
                    }
                }
            }
        stage("deploy") {
            steps {
                script {
                    echo "deploying"
                    //gv.deployApp()
                }
            }
        }
    }   
}
