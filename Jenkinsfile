node {
    //def homemaven = /var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation
    
    def homemaven=tool name: "maven-3.9.9"

    stage('gitchekout'){
       git branch: 'hotfix', credentialsId: '01edf231-8fef-4a9f-af75-555848a3ec6d', url: 'https://github.com/sandhumanu/maven-web-app-project-kk-funda.git' 
    }
    stage('build'){
        sh "${homemaven}/bin/mvn clean package"
    }
    stage('sq report'){
     sh "${homemaven}/bin/mvn clean sonar:sonar"
    }
    stage('artifact to nexus'){
        sh "${homemaven}/bin/mvn clean deploy"
    }
    stage('deploy tomcat'){
     sshagent(['fa6d200a-333d-4c49-8541-44b346448735']) {
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.110.175.253:/opt/apache-tomcat-9.0.98/webapps/"
     }
    }  
     post {
        success {
            slackSend(
                channel: '#devops-channel',
                message: "Pipeline ${env.JOB_NAME} succeeded at ${env.BUILD_URL}"
            )
        }
        failure {
            slackSend(
                channel: '#devops-channel',
                message: "Pipeline ${env.JOB_NAME} failed at ${env.BUILD_URL}"
            )
        }
        always {
            slackSend(
                channel: '#devops-channel',
                message: "Pipeline ${env.JOB_NAME} completed. See details at ${env.BUILD_URL}"
            )
        }
    }
}
