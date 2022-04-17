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
		stage('Unit Test') {
            steps {
                junit '**/target/surefire-reports/TEST-*.xml'
                archiveArtifacts 'target/*.jar'
//                hygieiaBuildPublishStep buildStatus: 'Success'
//				hygieiaArtifactPublishStep artifactDirectory: './//target', artifactGroup: 'test', artifactName: '*.jar', artifactVersion: ''
//                hygieiaDeployPublishStep applicationName: 'Devops3', artifactDirectory: './target', artifactGroup: 'test', artifactName: '*.jar', artifactVersion: '', buildStatus: 'Success', environmentName: 'Dev'
				hygieiaDeployPublishStep applicationName: 'develop-pipeline', artifactDirectory: 'target', artifactGroup: 'com.example', artifactName: '*.war', artifactVersion: '', buildStatus: 'Success', environmentName: 'DEV'   
				hygieiaDeployPublishStep applicationName: 'develop-pipeline', artifactDirectory: 'target', artifactGroup: 'com.example', artifactName: '*.war', artifactVersion: '', buildStatus: 'Success', environmentName: 'QA'
				hygieiaDeployPublishStep applicationName: 'develop-pipeline', artifactDirectory: 'target', artifactGroup: 'com.example', artifactName: '*.war', artifactVersion: '', buildStatus: 'Success', environmentName: 'PROD'    
				hygieiaCodeQualityPublishStep checkstyleFilePattern: '**/*/checkstyle-result.xml', findbugsFilePattern: '**/*/Findbugs.xml', jacocoFilePattern: '**/*/jacoco.xml', junitFilePattern: '**/*/TEST-.*-test.xml', pmdFilePattern: '**/*/PMD.xml'
 
            }
        }
		stage('Integration Test') {
            steps {
                sh "mvn -Dmaven.test.failure.ignore=true clean verify"
            }
        }
		stage('SonarQube Analysis') {
			steps {
			def mvn = tool 'Default Maven'
			withSonarQubeEnv() {
				sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=coe-hygieia"
			}
		}
	}
}
}