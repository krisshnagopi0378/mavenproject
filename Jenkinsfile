node {
 stage('Checkout the dockefile from GitHub') {            
        git branch: 'master', credentialsId: 'github', url: 'git@github.com:USBharath/mavenproject.git'        
      }   
     stage('Build') { 
           sh 'mvn clean package' 
                  }

    stage('Build the Image and Push to Azure Container Registry') {                
      app = docker.build('maventest.azurecr.io/bookstore')                
      withDockerRegistry([credentialsId: 'acr-credentials', url: 'https://maventest.azurecr.io']) {                
      app.push("${env.BUILD_NUMBER}")                
      app.push('latest')                
      }        
     }        
     
     stage('Delpoying the App on Azure Kubernetes Service') {            
        sh "kubectl set image deployment/azure-bookstore azure-bookstore=maventest.azurecr.io/bookstore:${env.BUILD_NUMBER}"            
    }    


}
