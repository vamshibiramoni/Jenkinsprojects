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
         
         sh "mkdir /var/www/html/rectangles/all/${env.BRANCH_NAME}" 
         
         sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/${env.BRANCH_NAME}/"
         
           }
         }
    stage("Running on CentOS"){
    
    agent {
    label 'CentOS'
     
    }
    
    steps{
    
    sh "wget http://52.32.173.250/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.BUILD_NUMBER}.jar"
    sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 4 4 "
    
    }
    
    }
    
    stage("Test On Docker"){
    
    agent {
    
    docker 'openjdk:8u171-jre'
     
    }
    
    steps{
    
    sh " wget http://52.32.173.250/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.BUILD_NUMBER}.jar "
    
    sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 4 4 "
    }
    
      
    } 
    
    stage('promote to green'){
    
       agent{
       label 'apache'
       
       }
       
       when{
        
        branch 'master'
        
       }
        steps{
        
        sh "cp /var/www/html/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar  /var/www/html/rectangles/green/rectangle_${env.BUILD_NUMBER}.jar"
       
        }
    
    
    
    
    
        } 
        
        
        stage('promote development branch to master'){
        
            agent {
        
        label 'apache'
        
             }
             
             when {
             branch 'development'
             }
             
             steps{
             
             
             echo"tashing any local changes"
             sh 'git stash'
             echo "checking out development branch"
             sh 'git checkout development'
             echo "checking out master"
             sh 'git checkout master'
             echo "Merging devlopment into master"
             sh 'git merge development'
             echo "pushing to master"
             sh 'git push origin master'
        
             }
        
        }
 
     
     
    }
      
      
           
  
}