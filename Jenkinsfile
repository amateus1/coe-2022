pipeline {
    agent any
	stages {
        stage('Preparation') {
            steps {
                git 'https://github.com/jglick/simple-maven-project-with-tests.git'
				//environment {
					sh '$JAVA_HOME/usr/lib/jvm/java-1.11.0-openjdk-amd64'
					//env.JAVA_HOME = "${jdk}"
					sh "java -version"
					echo "jdk installation path is: ${jdk}"
					// next 2 are equivalents
					//sh "${jdk}/bin/java -version"
					// note that simple quote strings are not evaluated by Groovy
					// substitution is done by shell script using environment
					//sh '$JAVA_HOME/bin/java -version'
				//}
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
//		stage('SonarQube Analysis') {
//			steps {
//			def mvn = tool 'Default Maven'
//			withSonarQubeEnv() {
//				sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=coe-hygieia"
//			}
//		}
//	}
		stage('Sonar') {
			steps {
				sh "java -version"
				sh "mvn sonar:sonar -Dsonar.projectKey=coe-hygieia   -Dsonar.host.url=http://mep-sonar.eastus2.cloudapp.azure.com  -Dsonar.login=ef026f77b563ee37ea01bb630b4dc2701ce4a306"
				hygieiaSonarPublishStep ceQueryIntervalInSeconds: '10', ceQueryMaxAttempts: '30'
			} 
		}
	}
}



