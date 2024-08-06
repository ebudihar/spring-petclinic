pipeline {
    agent { label 'linux-01' }
    stages {
        stage ('Checkout') {
          steps {
            git 'https://github.com/effectivejenkins/spring-petclinic.git'
          }
        }
        stage('Build') {
            agent { docker 'maven:3.5-alpine' }
            steps {
                sh 'mvn clean package'
                junit '**/target/surefire-reports/TEST-*.xml'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
        stage('Deploy') {
          steps {
            input 'Do you approve the deployment?'
            sh 'scp /home/jenkins/workspace/petclinic_pipeline1@2/target/*.jar jenkins@10.10.13.136:/opt/pet/'
            //sh "ssh jenkins@10.10.14.140 'nohup java -jar /opt/pet/spring-petclinic-1.5.1.jar &'"
          }
        }
    }
}
