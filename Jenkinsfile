node {

    stage('SCM Checkout'){
		git 'https://github.com/Madhunirmal/webapp-cicd-project.git'
	}
	
	stage('maven-buildstage'){
	
	  def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
	  
	  }
	  
	stage('docker image build'){
	   sh 'docker build -t madhunirmal/webapp:0.0.2 .'
	   }
	   
	stage('docker image push'){
	      withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
          sh "docker login -u madhunirmal -p ${dockerPassword}"
    }
           sh 'docker push madhunirmal/webapp:0.0.2'
   }
   
         stage('Nexus Image Push'){
   sh "docker login -u admin -p admin123 54.89.228.11:8083"
   sh "docker tag madhunirmal/webapp:0.0.2 54.89.228.11:8083/nirmal:1.0.0"
   sh 'docker push 54.89.228.11:8083/nirmal:1.0.0'
   }
   
      stage('Remove Previous Container'){
	  try{
		sh 'docker rm -f tomcattest'
	 }catch(error){
		//  do nothing if there is an exception
	}
}

   
   stage('docker deployment'){
		
		sh 'docker run -d -p 8080:8090 --name tomcattest madhunirmal/webapp:0.0.2'
   
   }
	
}
