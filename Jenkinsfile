pipeline {
    agent any
    environment {
    PATH = "/usr/local/src/apache-maven/bin:$PATH"
    }
    stages {
        stage('GitHub Clone') {
            steps {
                git branch: 'project02', url: 'https://github.com/Chey2266/project02.git'
            }
        }
        stage('Build Maven') {
            steps {
                sh "mvn clean install package"
            }
        }
        stage('Deploy Tomcat') {
            steps {
                deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: '3fe07e03-12fd-4199-8d53-2929a9dbfaf2',
                path: '',
                url: 'http://13.204.76.249:8080/')],
                contextPath: 'project02',
                war: '**/*.war'
            }
        }
        stage('add files') {
            steps {
                sh 'git add Jenkinsfile'
            }
        }
        
    }
    post {
        success {
            emailtext to: "cheym2266@gmail.com",
            recipientProviders: [developers()],
            subject: "jenkins Pipe :${currentBuild.currentResult}: ${env.JOB_NAME}",
            body: "${currentBuild.currentResult}: Job ${env.JOB_NAME}\n More Info can be found here: ${env.BUILD_URL}",

            attachLog: true
            
            slackSend meesage: "Build deployed successfully - Job ${env.JOB_NAME}\n More Info can be found here: ${env.BUILD_URL}"
        }
    }
}