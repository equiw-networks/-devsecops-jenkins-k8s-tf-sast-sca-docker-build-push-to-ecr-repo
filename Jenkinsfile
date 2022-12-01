pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=equiwbuggywebapp -Dsonar.organization=equiwbuggywebapp -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=fcfe33f5169a4afb95b253406ae55d5f54f8b116'
			}
    }

	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }

	stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("equiw")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://520288862384.dkr.ecr.ap-southeast-3.amazonaws.com', 'ecr:ap-southeast-3:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}
	    
  }
}
