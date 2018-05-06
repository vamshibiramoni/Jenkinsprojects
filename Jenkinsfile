pipeline{

   agent none
   
   environment {
       MAJOR_VERSION = 1
   }

   
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
         
         
         
         sh "if ![ -d "/var/www/html/rectangles/all/${env.BRANCH_NAME}"]; then mkdir /var/www/html/rectangles/all/${env.BRANCH_NAME};fi" 
         
         sh "cp dist/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/${env.BRANCH_NAME}/"
         
           }
         }
    stage("Running on CentOS"){
    
    agent {
    label 'CentOS'
     
    }
    
    steps{
    
    sh "wget http://34.218.41.120/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar"
    sh "java -jar rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar 4 4 "
    
    }
    
    }
    
    stage("Test On Docker"){
    
    agent {
    
    docker 'openjdk:8u171-jre'
     
    }
    
    steps{
    
    sh " wget http://34.218.41.120/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar "
    
    sh "java -jar rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar 4 4 "
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
        
        sh "cp /var/www/html/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar  /var/www/html/rectangles/green/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar"
       
         echo "Taging the release "
           sh "git tag rectangle-${env.MAJOR_VERSION}.${env.BUILD_NUMBER} "
           
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
             sh 'git pull origin'
             sh 'git checkout master'
             echo "Merging devlopment into master"
             sh 'git merge development'
             echo "pushing to master"
           
         
           // sh 'git push origin master'
           echo "Taging the release "
           sh "git tag rectangle-${env.MAJOR_VERSION}.${env.BUILD_NUMBER} "
         //  sh "git push origin rectangle-${env.MAJOR_VERSION}.${env.BUILD_NUMBER}" 
        
             }
             
             
        
        }
 
     
    }
      
      
           
  
}