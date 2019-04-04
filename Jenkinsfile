node{
      
      stage('SCM Checkout'){
         git 'https://github.com/hkalyana/Java-Demo-Application'
            
      }
      
      stage('Build'){
         // Get maven home path and build
         def mvnHome =  tool name: 'Maven 3.5.4', type: 'maven'   
         sh "${mvnHome}/bin/mvn package -Dmaven.test.skip=true"
      }       
       
      stage ('Test'){
         def mvnHome =  tool name: 'Maven 3.5.4', type: 'maven'    
         sh "${mvnHome}/bin/mvn verify; sleep 3"
      }
      
      stage('Build Docker Image'){
         sh 'docker build -t hkalyana/javademoapp_$JOB_NAME:$BUILD_NUMBER .'
      }  
   
      stage('Publish Docker Image'){
         withCredentials([string(credentialsId: 'dockerpwdhkalyana', variable: 'dockerpwdhkalyana')]) {
              sh "docker login -u hkalyana -p ${dockerpwdhkalyana}"
         }
        sh 'docker push hkalyana/javademoapp_$JOB_NAME:$BUILD_NUMBER'
        sh "sed -i.bak 's/#BUILD-NUMBER#/$BUILD_NUMBER/' deployment.yaml"
        sh "sed -i.bak 's/#JOB-NAME#/$JOB_NAME/' deployment.yaml"
      }
          
    /*
      // ********* For Azure Cluster**************************
      stage('Deploy'){
         def k8Apply= "kubectl apply -f deployment.yaml" 
         withCredentials([string(credentialsId: 'k8pwd', variable: 'k8PWD')]) {
          sh "sshpass -p ${k8PWD} ssh -o StrictHostKeyChecking=no ubuntu@104.211.154.236" 
          sh "sshpass -p ${k8PWD} scp -r deployment.yaml ubuntu@104.211.154.236:/home/ubuntu" 
          sh "sshpass -p ${k8PWD} ssh  -o StrictHostKeyChecking=no ubuntu@104.211.154.236 ${k8Apply}"
         }
       }
      */
         
       
    
       
      // ********* For AWS Cluster**************************
      stage('Deploy'){
         def k8Apply= "kubectl apply -f deployment.yaml" 
         withCredentials([string(credentialsId: 'haproxyhkalyana', variable: 'haproxyhkalyana')]) {
             sh "sshpass -p ${haproxyhkalyana} ssh -o StrictHostKeyChecking=no harish@52.187.5.138"  
             sh "sshpass -p ${haproxyhkalyana} scp -r deployment.yaml harish@52.187.5.138:/home/harish" 
             sh "sshpass -p ${haproxyhkalyana} ssh -o StrictHostKeyChecking=no harish@52.187.5.138 ${k8Apply}"
         }
       }
        
  }
