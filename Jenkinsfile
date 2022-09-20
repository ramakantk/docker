
node {
    
    def app
  
    stage('Initialize'){
        def dockerHome = tool 'myDocker'
        env.PATH = "${dockerHome}/bin:${env.PATH}"
       echo env.PATH
          
    }
  
    stage('Clone repository') {
      

        checkout scm
    }

    stage('Build image') {
  
       app = docker.build("mailtoramakant/test")
    }

    stage('Test image') {
  

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

 
  
  
    stage('Push image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'docker') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
  
   stage('sign the image') {
     
      sh 'cosign version'
     withCredentials([file(credentialsId: 'cosign-private-key', variable: 'COSIGN_PRIVATE_KEY')]) {
        
         sh 'cosign sign --key $COSIGN_PRIVATE_KEY mailtoramakant/test:latest'
    }
   }
     
     
        
}
