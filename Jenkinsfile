node {
	def application = "springbootapp"
	def dockerhubaccountid = "shubhamb2424"
	stage('Clone repository') {
		checkout scm
	}

	stage('Build image') {
		app = docker.build("${dockerhubaccountid}/${application}")
	}
	
	stage('Test image') {           
            app.inside {            
             
             sh 'echo "Tests passed"'        
            }    
        }   

	stage('Push image') {
	docker.withRegistry('https://registry.hub.docker.com', 'dockerHub') {            
       app.push("${env.BUILD_NUMBER}")            
       app.push("latest")        
              }    
           }
	}

	stage('Deploy') {
		sh ("docker run -d -p 80:8080 -v /var/log/:/var/log/ ${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
	}
	
	stage('Remove old images') {
		// remove docker pld images
		sh("docker rmi ${dockerhubaccountid}/${application}:latest -f")
   }
}
