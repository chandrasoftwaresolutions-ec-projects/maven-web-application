node('master'){
    def MavenHome = tool name:"maven3.8.1"
    stage('CheckoutCode'){
        git branch: 'development', credentialsId: '7ce74b0a-d4db-47b0-a88d-b062abd02433', url: 'https://github.com/chandrasoftwaresolutions-ec-projects/maven-web-application.git'
    }
    stage('Build'){
       sh "${MavenHome}/bin/mvn clean package"
    }
    stage('ExecuteSonarQubeReport'){
        sh "${MavenHome}/bin/mvn sonar:sonar"
    }
    stage('UploadArtifactIntoNexus'){
        sh "${MavenHome}/bin/mvn deploy"
    }
    stage('DeployAppIntoTomcatSever'){
        sshagent(['2b428101-a6f4-4326-87ea-f7da96ada906']) {
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.233.192.215:/opt/apache-tomcat-9.0.50/webapps/"
    }
    }
    stage('SendEmailNotification'){
        emailext body: '''Build Success.....

    Regards,
    chandrasekhar.''', subject: 'Build Success', to: 'al.chandrasekhar996@gmail.com,chandrasekharoyal@gmail.com'
    }
    
}
