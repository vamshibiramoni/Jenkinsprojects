pipeline{

   agent none
   
   options{
   
   buildDiscarder(logRotator(numToKeepStr: '2' , artifactNumToKeepStr: '1'))
   
   }
   
   

     stages{
     
      /* stage ('Unit Tests') {
       
         steps {
         
         sh 'ant -f test.xml -v'
         
         junit 'reports/result.xml'
         
       
         }
       
       }
       */

        stage('build'){
        
        agent {
         
         label 'apache'
        }
     
            steps {
     
               sh 'ant -f build.xml -v'
                                              
                  }   
                  
                  
               post {
            
                   success{
            
                     archiveArtifacts artifacts: 'dist/*.jar' , fingerprint: true
              
                    }
                }

                  
     
         }
         
         
         stage('deploy'){
           agent {
           
           label 'apache'
           }
         steps{
         
         sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/"
         
           }
         }
    stage("Running on CentOS"){
    
    agent {
    label 'CentOS'
     
    }
    
    steps{
    
    sh "wget http://18.236.254.158/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar"
    sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 4 4 "
    
    }
    
    }
    
    stage("Test On Docker"){
    
    agent {
    
    docker 'openjdk:8u171-jre'
     
    }
    
    steps{
    
    sh "cp wget http://18.236.254.158/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar "
    
    sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 4 4 "
    }
    
      
    } 
    
     
     
     
      }
      
      
           
  
}