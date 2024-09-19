pipeline{
    agent any
    tools{
        maven 'Maven'
        nodejs 'node16'
    }
    stages {
        stage('clean workspace'){
            steps{
                cleanWs()
            }
        }
        stage("Sonarqube Analysis "){
            steps{
               sh "mvn clean verify sonar:sonar -Dsonar.projectKey=Netflix -Dsonar.projectName='Netflix' -Dsonar.host.url=http://172.21.21.122:9000 -Dsonar.token=sqp_296e3049f694920f9b9552e2ea8be16735168e36"
            }
        }
        stage("quality gate"){
           steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
    post {
     always {
        emailext attachLog: true,
            subject: "'${currentBuild.result}'",
            body: "Project: ${env.JOB_NAME}<br/>" +
                "Build Number: ${env.BUILD_NUMBER}<br/>" +
                "URL: ${env.BUILD_URL}<br/>",
            to: 'bvcious10@gmail.com',
            attachmentsPattern: 'trivyfs.txt,trivyimage.txt'
        }
    }
}
