pipeline {
    agent any
	stages {
        stage('Preparation') {
            steps {
                git 'https://github.com/jglick/simple-maven-project-with-tests.git'
            }
        }
        stage('Build') {
            steps {
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
	stage('Results') {
            steps {
                junit '**/target/surefire-reports/TEST-*.xml'
                archiveArtifacts 'target/*.jar'
                hygieiaBuildPublishStep buildStatus: 'Success'
				//hygieiaArtifactPublishStep artifactDirectory: './target', artifactGroup: 'test', artifactName: '*.jar', artifactVersion: ''
                //hygieiaDeployPublishStep applicationName: 'Devops3', artifactDirectory: './target', artifactGroup: 'test', artifactName: '*.jar', artifactVersion: '', buildStatus: 'Success', environmentName: 'Dev'
            }
        }
    }
}