pipeline {
  agent any 
             
  tools {
    maven 'maven'
  }
        
  environment {
    Snyk = 'Snyk'
    Trivy = 'Trivy'
    Audit = 'Audit'
  }
  
  stages {
    stage ('Initialize') {
      steps {
        sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                  
            ''' 
      }
    } 
    
     
 //stage ('Check-Git-Secrets') {
      //steps {
        //sh 'docker run gesellix/trufflehog --json https://github.com/st-abhay/VulnerableApp.git > trufflehog'
        //sh 'cat trufflehog'
      //}
    //}  
    
    
  stage ('SCA') {
       steps {
             dependencyCheck additionalArguments: ''' 
              -o "./" 
              -s "./"
              -f "ALL" 
              --prettyPrint''', odcInstallation: 'dcheck'
              dependencyCheckPublisher pattern: 'dependency-check-report.xml'
              sh 'cat dependency-check-report.xml'
              sh 'ls'
              sh 'curl -X POST "http://10.66.173.133:8080/api/v2/import-scan/" -H "accept: application/json" -H "Authorization: Token 779ceb6db281a9dbe41ef48bbbc2e8d925b9b2dd" -H "Content-Type: multipart/form-data" -H "X-CSRFToken: hdec09ZzEDciBQsZqTgXHKJt97jTKyoipLplwQ7sgCO5n0xPP6Z0DhhzwRVMIyJ0" -F "minimum_severity=Info" -F "active=true" -F "verified=true" -F "scan_type=Dependency Check Scan" -F "file=@dependency-check-report.xml;type=text/xml" -F "product_name=ST-DevSecOps" -F "engagement_name=ST-DevSecOps" -F "close_old_findings=false" -F "push_to_jira=false"'         
            }
        }       
    
    stages {
    stage('SAST') {
        steps {
            sh 'curl -fsSL https://raw.githubusercontent.com/ZupIT/horusec/main/deployments/scripts/install.sh | bash -s latest'
            sh 'horusec start -p="./" -e="true"'
        }
    }
          
    
 stage ('AppScan Cloud SAST') {
         steps {
            appscan application: '4130440a-8227-4b5f-b846-e4ef704931fb', credentials: 'HCLEnt', name: 'DevSecOps', scanner: static_analyzer(hasOptions: false, target: '/var/lib/jenkins/workspace/DevSecOps/'), type: 'Static Analyzer'
      } 
    }
    
   
   stage ('Build') {
      steps {
      sh 'mvn clean package'
     }
    }

stage ('Container Scan') {
       steps {
            sh 'docker run aquasec/trivy:0.18.3 vulnerables/phpldapadmin-remote-dump'
            sh 'docker run aquasec/trivy:0.18.3 repo https://github.com/st-abhay/VulnerableApp.git'
            sh 'docker run aquasec/trivy:0.18.3 image vulnerables/web-dvwa:latest'
             }
             }  
      
    stage ('Deploy') {
     steps {
            echo test
       }
    }
      
  stage ('Appscan Cloud DAST') {
      steps {
      appscan application: '4130440a-8227-4b5f-b846-e4ef704931fb', credentials: 'HCLEnt', name: 'DevSecOps_DAST', scanner: dynamic_analyzer(hasOptions: false, loginType: 'None', optimization: 'Fast', scanType: 'Staging', target: 'http://altoromutual.com:8080/login.jsp'), type: 'Dynamic Analyzer'
     } 
    } 
 
    
  }
}
