 node {
 stage('Checkout the sourcecode from GitHub') {            
        git branch: 'master', credentialsId: 'github', url: 'git@github.com:USBharath/mavenproject.git'        
      }   
     stage('Build') { 
           sh 'mvn clean package' 
                  }

    stage('Build the Image and Push to Azure Container Registry') {                
      app = docker.build('maventest.azurecr.io/bookstore')                
      withDockerRegistry([credentialsId: 'acr-credentials', url: 'https://maventest.azurecr.io']) {                
      app.push("build-${env.BUILD_NUMBER}")                
                     
      }        
     } 

     stage('Delpoying the App on Azure Kubernetes Service') {            
         kubernetesDeploy(kubeconfigId: 'azure-kubeconfig', configs: 'kubernet.yaml')            
    }  
    
   
                 


}
