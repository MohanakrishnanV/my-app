node{
   stage('SCM Checkout'){
     git 'https://github.com/MohanakrishnanV/my-app.git'
   }
   stage('Compile-Package'){
      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
	   	sh 'sudo chmod 777 -R /var/run/docker.sock'
   }
   stage('Build Docker Imager'){
   sh 'docker build -t mohan29/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u mohan29 -p ${dockerPassword}"
    }
   sh 'docker push mohan29/myweb:0.0.2'
   }
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   }
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest mohan29/myweb:0.0.2' 
   }
}
