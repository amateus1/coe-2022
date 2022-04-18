pipeline {
    agent any
	tools {
        jdk 'JDK-8'
    }
	stages {
		stage('jdk 11') {
			steps {
			withEnv(["JAVA_HOME=${tool 'JDK-8'}", "PATH=${tool 'JDK-8'}/bin:${env.PATH}"]) {
			sh 'java -version'
			sh 'javac -version'
        }
      }
    }

        stage('Preparation') {
            steps {
                git 'https://github.com/amateus1/simple-maven-project-with-tests.git'
				//git 'https://github.com/jglick/simple-maven-project-with-tests.git'
				//environment {
					//sh "export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64"
					sh "export PATH=$JAVA_HOME/bin:$PATH"
					sh "export JAVA_HOME"
					sh "export JRE_HOME"
					sh "export PATH"

//					sh '$JAVA_HOME = /usr/lib/jvm/java-1.11.0-openjdk-amd64'
					//env.JAVA_HOME = "${jdk}"
					sh "java -version"
					//echo "jdk installation path is: ${jdk}"
					// next 2 are equivalents
					//sh "${jdk}/bin/java -version"
					// note that simple quote strings are not evaluated by Groovy
					// substitution is done by shell script using environment
					//sh '$JAVA_HOME/bin/java -version'
				//}

				sh "java -version"
            }
        }
        stage('Build') {
            steps {
				sh "echo '**** STARTIN BUILD ******'"
				sh "java -version"                
				sh "echo 'coe+best2022' | sudo -S mvn -Dmaven.test.failure.ignore=true clean install package"
				sh "java -version"
				sh "echo '**** END BUILD ******'"
            }
        }
		stage('Unit Test') {
            steps {
				sh "java -version"                
				sh "echo '**** STARTIN TEST ******'"
				junit '**/target/surefire-reports/TEST-*.xml'
                archiveArtifacts 'target/*.jar'
				sh "echo '**** COMPLETED TEST ******'"
				sh "pwd"
				sh "cd target"
				sh "cd target"
				sh "ls"
				sh "echo '**** STARTING TEST ###2 ******'"
				sh "echo 'coe+best2022' | sudo -S mvn -Dmaven.test.failure.ignore verify"
				sh "echo '**** COMPLETED TEST ###2 ******'"
//				sh "ls /target/surefire-reports/"
                hygieiaBuildPublishStep buildStatus: 'Success'
//				hygieiaArtifactPublishStep artifactDirectory: './//target', artifactGroup: 'test', artifactName: '*.jar', artifactVersion: ''
//                hygieiaDeployPublishStep applicationName: 'Devops3', artifactDirectory: './target', artifactGroup: 'test', artifactName: '*.jar', artifactVersion: '', buildStatus: 'Success', environmentName: 'Dev'
				hygieiaDeployPublishStep applicationName: 'develop-pipeline', artifactDirectory: 'target', artifactGroup: 'com.example', artifactName: '*.jar', artifactVersion: '', buildStatus: 'Success', environmentName: 'DEV'   
				hygieiaDeployPublishStep applicationName: 'develop-pipeline', artifactDirectory: 'target', artifactGroup: 'com.example', artifactName: '*.jar', artifactVersion: '', buildStatus: 'Success', environmentName: 'QA'
				hygieiaDeployPublishStep applicationName: 'develop-pipeline', artifactDirectory: 'target', artifactGroup: 'com.example', artifactName: '*.jar', artifactVersion: '', buildStatus: 'Success', environmentName: 'PROD'    
//				hygieiaCodeQualityPublishStep checkstyleFilePattern: '**/*/checkstyle-result.xml', findbugsFilePattern: '**/*/Findbugs.xml', jacocoFilePattern: '**/*/jacoco.xml', junitFilePattern: '**/*/TEST-.*-test.xml', pmdFilePattern: '**/*/PMD.xml'
 				sh "java -version"
				sh "echo '**** COMPLETED TEST ******'"
            }
        }
		stage('Integration Test') {
            steps {
                sh "java -version"
				sh "echo 'coe+best2022' | sudo -S mvn -Dmaven.test.failure.ignore=true clean verify"
				sh "java -version"				
            }
        }
  stage('SonarQube Analysis') {
//    def mvn = tool 'Default Maven';

    tools {
        jdk "JDK-11" // the name you have given the JDK installation using the JDK manager (Global Tool Configuration)
    }
	environment {
        scannerHome = tool 'SONAR' // the name you have given the Sonar Scanner (Global Tool Configuration)
    }
    steps {
		sh "echo '**** STARTING SONAR TEST 2******'"
        withSonarQubeEnv(installationName: 'SONAR') {
            sh "echo 'coe+best2022' | sudo -S mvn sonar:sonar -Dsonar.host.url=http://mep-sonar.eastus2.cloudapp.azure.com -Dsonar.login=fe5b9d9f8a95064ec4a4547c850700dd78c1b038"
        }
    sh "echo '**** FINISHED SONAR TEST ******'"
	  }
    }		
	
//		stage('SonarQube Analysis') {
//			steps {
//			def mvn = tool 'Default Maven'
//			sh "java -version"
//			sh "echo '**** STARTING SONAR ******'"
//			withSonarQubeEnv('SONAR') {
//				sh "echo 'coe+best2022' | sudo -S mvn install  sonar:sonar -Dsonar.projectKey=coe-hygieia -Dsonar.host.url=http://mep-sonar.eastus2.cloudapp.azure.com  -Dsonar.login=fe5b9d9f8a95064ec4a4547c850700dd78c1b038"
//			sh "echo '**** SONAR DONE ******'"	
//			}
//		}
//	}
//		stage('Sonar') {
//			steps {
//				sh "java -version"
//				//sh "export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64"
//				sh "export PATH=$JAVA_HOME/bin:$PATH"
//				sh "export JAVA_HOME"
//				sh "export JRE_HOME"
//				sh "export PATH"
//				sh "java -version"
//				sh "echo '**** STARTING VERIFY ******'"
//				sh "echo 'coe+best2022' | sudo -S mvn clean install verify sonar:sonar -Dsonar.projectKey=coe-hygieia   -Dsonar.host.url=http://mep-sonar.eastus2.cloudapp.azure.com  -Dsonar.login=fe5b9d9f8a95064ec4a4547c850700dd78c1b038"
//				sh "echo '**** ENDED VERIFY ******'"
//				sh "echo '**** STARTING CLEAN INSTALL ******'"
//				sh "echo 'coe+best2022' | sudo -S mvn clean install"
//				sh "echo '**** ENDED CLEAN INSTALL ******'"
//				sh "echo 'coe+best2022' | sudo -S mvn sonar:sonar -Dsonar.projectKey=coe-hygieia   -Dsonar.host.url=http://mep-sonar.eastus2.cloudapp.azure.com  -Dsonar.login=fe5b9d9f8a95064ec4a4547c850700dd78c1b038"
//				hygieiaSonarPublishStep ceQueryIntervalInSeconds: '10', ceQueryMaxAttempts: '30'
//			} 
//		}
	}
}



