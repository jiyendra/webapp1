node{
   stage('SCM Checkout'){
     git 'https://github.com/jiyendra/webapp1.git'
   }
   stage('Compile-Package'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }
   stage('Build Docker Image'){
   sh 'docker build -t cube45/myweb:1 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u cube45 -p ${dockerPassword}"
    }
   sh 'docker push cube45/myweb:1'
   }
   stage('Nexus Image Push'){
   sh "docker login -u admin -p admin123 4.45.110.45:8085"
   sh "docker tag cube45/myweb:1 4.45.110.45:8085/myweb:1"
   sh 'docker push 3.110.204.26:8083/myweb:1'
   }
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest cube45/myweb:1' 
   }
   }
}
