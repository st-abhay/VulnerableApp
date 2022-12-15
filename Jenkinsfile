pipeline {
  agent any 
     
 // options//{
   //     ansiColor('xterm')
    //}
        
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
    
     
 stage ('Check-Git-Secrets') {
      steps {
        sh 'sudo chmod 666 /var/run/docker.sock'
        //sh 'sudo docker run raphaelareya/gitleaks:latest -r https://github.intra.fcagroup.com/t0996su/CICD.git'
        // sh 'sshpass -p Stellantis01 ssh devuser@10.109.137.30 "exit 0" '
        
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/cehkunal/webapp.git > trufflehog'
        sh 'cat trufflehog'
      }
    }  
    
    
  stage ('DJScan') {
       steps {
             dependencyCheck additionalArguments: ''' 
              -o "./" 
              -s "./"
              -f "ALL" 
              --prettyPrint''', odcInstallation: 'dcheck'
              dependencyCheckPublisher pattern: 'dependency-check-report.xml'       
              sh 'sudo cp -r /var/lib/jenkins/workspace/DJScan/./dependency-check-report.html  /var/www/html/DevSecOps'         
            }
        }       
         
    stage ('Container Scan') {
       steps {
            sh 'docker run aquasec/trivy:0.18.3 vulnerables/phpldapadmin-remote-dump'
            sh 'docker run aquasec/trivy:0.18.3 repo https://github.intra.fcagroup.com/t0996su/CICD.git'
            sh 'docker run aquasec/trivy:0.18.3 image vulnerables/web-dvwa:latest'
             }
             } 
   
/* stage ('HCL Ent') {
         steps {
           
            appscanenterprise application: 'test', credentials: 'HCLEnt', folder: 'test', jobName: 'test', loginType: 'None', scanType: '1', target: 'www.demo.testfire.net', template: 'test', testOptimization: '2', testPolicy: ''
      } 
    }*/

    
    
   
    
    
 /* stage ('Burp Scanner') {
         steps {
         //sh 'curl -vgw "\\n" -X POST \'http://192.168.193.1:8080/api/bThqBIr53GwDSRkFU8rezwvifYdhuUlG/v0.1/scan\' -d \'{"application_logins":[{"label":"7389_Login","script":"[     {         \\"name\\": \\"Burp Suite Navigation Recorder\\",         \\"version\\": \\"1.4.19\\",         \\"eventType\\": \\"start\\",         \\"platform\\": \\"Win32\\",         \\"iframes\\": [],         \\"windows\\": [             {                 \\"windowId\\": 229             }         ],         \\"tabs\\": [             {                 \\"tabId\\": 230,                 \\"windowId\\": 229             }         ]     },     {         \\"date\\": \\"2022-07-04T16:20:57.460Z\\",         \\"timestamp\\": 1656951657460,         \\"eventType\\": \\"goto\\",         \\"url\\": \\"https://fcamvptest.intra.chrysler.com/FGAPD/\\",         \\"triggersNavigation\\": true,         \\"frameId\\": 0,         \\"tabId\\": 230,         \\"windowId\\": 229,         \\"fromAddressBar\\": true     },     {         \\"date\\": \\"2022-07-04T16:21:01.455Z\\",         \\"timestamp\\": 1656951661455,         \\"windowInnerWidth\\": 1366,         \\"windowInnerHeight\\": 625,         \\"uniqueElementID\\": \\"7v3egcz0gxg-bcn7qf3xa65-vkx2rt8gji\\",         \\"tagName\\": \\"INPUT\\",         \\"eventType\\": \\"click\\",         \\"placeholder\\": \\"\\",         \\"tagNodeIndex\\": 2,         \\"className\\": \\"\\",         \\"name\\": \\"USER\\",         \\"id\\": \\"user-id\\",         \\"frameId\\": 0,         \\"tabId\\": 230,         \\"textContent\\": \\"\\",         \\"innerHTML\\": \\"\\",         \\"value\\": \\"\\",         \\"elementType\\": \\"text\\",         \\"xPath\\": \\"/html/body/div/div/div/form/div/ul/li[2]/input\\",         \\"triggersNavigation\\": false,         \\"triggersWithinDocumentNavigation\\": false,         \\"characterPos\\": 0,         \\"shiftKey\\": false,         \\"ctrlKey\\": false,         \\"altKey\\": false,         \\"metaKey\\": false,         \\"windowId\\": 229,         \\"url\\": \\"https://login-test.chrysler.com/siteminderagent/forms/chryslerlogin.fcc?TYPE=33554433&REALMOID=06-5c594b4c-593f-10de-a09d-84fa13f20000&GUID=&SMAUTHREASON=0&METHOD=GET&SMAGENTNAME=-SM-a3lUnWazCsrbaYmfu6dxAODiSZMorSqXFnaD2MwBbbo%2bOa62sQs9JDhjaTaKb0Sea2P6G2UgxOSmgMr5ThqvINJeMloQYmbB&TARGET=-SM-https%3a%2f%2ffcamvptest%2eintra%2echrysler%2ecom%2fFGAPD%2f\\",         \\"isIframe\\": false     },     {         \\"date\\": \\"2022-07-04T16:21:02.036Z\\",         \\"timestamp\\": 1656951662036,         \\"windowInnerWidth\\": 1366,         \\"windowInnerHeight\\": 625,         \\"uniqueElementID\\": \\"ed0nb1zjpbs-3lifmvztkr-cuq2t0rg9yk\\",         \\"tagName\\": \\"INPUT\\",         \\"eventType\\": \\"typing\\",         \\"placeholder\\": \\"\\",         \\"tagNodeIndex\\": 2,         \\"className\\": \\"\\",         \\"name\\": \\"USER\\",         \\"id\\": \\"user-id\\",         \\"frameId\\": 0,         \\"tabId\\": 230,         \\"textContent\\": \\"\\",         \\"innerHTML\\": \\"\\",         \\"value\\": \\"\\",         \\"elementType\\": \\"text\\",         \\"xPath\\": \\"/html/body/div/div/div/form/div/ul/li[2]/input\\",         \\"triggersNavigation\\": false,         \\"triggersWithinDocumentNavigation\\": false,         \\"typedValue\\": \\"t0996su\\",         \\"windowId\\": 229,         \\"url\\": \\"https://login-test.chrysler.com/siteminderagent/forms/chryslerlogin.fcc?TYPE=33554433&REALMOID=06-5c594b4c-593f-10de-a09d-84fa13f20000&GUID=&SMAUTHREASON=0&METHOD=GET&SMAGENTNAME=-SM-a3lUnWazCsrbaYmfu6dxAODiSZMorSqXFnaD2MwBbbo%2bOa62sQs9JDhjaTaKb0Sea2P6G2UgxOSmgMr5ThqvINJeMloQYmbB&TARGET=-SM-https%3a%2f%2ffcamvptest%2eintra%2echrysler%2ecom%2fFGAPD%2f\\",         \\"isIframe\\": false     },     {         \\"date\\": \\"2022-07-04T16:21:06.004Z\\",         \\"timestamp\\": 1656951666004,         \\"eventType\\": \\"keyboard\\",         \\"shiftKey\\": false,         \\"ctrlKey\\": false,         \\"altKey\\": false,         \\"metaKey\\": false,         \\"key\\": \\"Tab\\",         \\"charCode\\": 0,         \\"frameId\\": 0,         \\"windowId\\": 229,         \\"url\\": \\"https://login-test.chrysler.com/siteminderagent/forms/chryslerlogin.fcc?TYPE=33554433&REALMOID=06-5c594b4c-593f-10de-a09d-84fa13f20000&GUID=&SMAUTHREASON=0&METHOD=GET&SMAGENTNAME=-SM-a3lUnWazCsrbaYmfu6dxAODiSZMorSqXFnaD2MwBbbo%2bOa62sQs9JDhjaTaKb0Sea2P6G2UgxOSmgMr5ThqvINJeMloQYmbB&TARGET=-SM-https%3a%2f%2ffcamvptest%2eintra%2echrysler%2ecom%2fFGAPD%2f\\",         \\"isIframe\\": false     },     {         \\"date\\": \\"2022-07-04T16:21:09.404Z\\",         \\"timestamp\\": 1656951669404,         \\"windowInnerWidth\\": 1366,         \\"windowInnerHeight\\": 625,         \\"uniqueElementID\\": \\"mofdz14awqd-e8qgej57ld-h652wi85q6w\\",         \\"tagName\\": \\"INPUT\\",         \\"eventType\\": \\"typing\\",         \\"placeholder\\": \\"\\",         \\"tagNodeIndex\\": 3,         \\"className\\": \\"\\",         \\"name\\": \\"PASSWORD\\",         \\"id\\": \\"user-pwd\\",         \\"frameId\\": 0,         \\"tabId\\": 230,         \\"textContent\\": \\"\\",         \\"innerHTML\\": \\"\\",         \\"value\\": \\"\\",         \\"elementType\\": \\"password\\",         \\"xPath\\": \\"/html/body/div/div/div/form/div/ul/li[4]/input\\",         \\"triggersNavigation\\": false,         \\"triggersWithinDocumentNavigation\\": false,         \\"typedValue\\": \\"Surya@9876\\",         \\"windowId\\": 229,         \\"url\\": \\"https://login-test.chrysler.com/siteminderagent/forms/chryslerlogin.fcc?TYPE=33554433&REALMOID=06-5c594b4c-593f-10de-a09d-84fa13f20000&GUID=&SMAUTHREASON=0&METHOD=GET&SMAGENTNAME=-SM-a3lUnWazCsrbaYmfu6dxAODiSZMorSqXFnaD2MwBbbo%2bOa62sQs9JDhjaTaKb0Sea2P6G2UgxOSmgMr5ThqvINJeMloQYmbB&TARGET=-SM-https%3a%2f%2ffcamvptest%2eintra%2echrysler%2ecom%2fFGAPD%2f\\",         \\"isIframe\\": false     },     {         \\"date\\": \\"2022-07-04T16:21:17.512Z\\",         \\"timestamp\\": 1656951677512,         \\"windowInnerWidth\\": 1366,         \\"windowInnerHeight\\": 625,         \\"uniqueElementID\\": \\"64coqiqpo8-qvxo8tmz9qk-sbaj7a5icaa\\",         \\"tagName\\": \\"INPUT\\",         \\"eventType\\": \\"click\\",         \\"placeholder\\": \\"\\",         \\"tagNodeIndex\\": 4,         \\"className\\": \\"btn btn-default no-border\\",         \\"name\\": \\"\\",         \\"id\\": \\"login-btn\\",         \\"frameId\\": 0,         \\"tabId\\": 230,         \\"textContent\\": \\"\\",         \\"innerHTML\\": \\"\\",         \\"value\\": \\"Sign in\\",         \\"elementType\\": \\"submit\\",         \\"xPath\\": \\"/html/body/div/div/div/form/div/ul/li[6]/input\\",         \\"ariaSelector\\": \\"aria/Sign in\\",         \\"triggersNavigation\\": true,         \\"triggersWithinDocumentNavigation\\": false,         \\"characterPos\\": null,         \\"shiftKey\\": false,         \\"ctrlKey\\": false,         \\"altKey\\": false,         \\"metaKey\\": false,         \\"windowId\\": 229,         \\"url\\": \\"https://login-test.chrysler.com/siteminderagent/forms/chryslerlogin.fcc?TYPE=33554433&REALMOID=06-5c594b4c-593f-10de-a09d-84fa13f20000&GUID=&SMAUTHREASON=0&METHOD=GET&SMAGENTNAME=-SM-a3lUnWazCsrbaYmfu6dxAODiSZMorSqXFnaD2MwBbbo%2bOa62sQs9JDhjaTaKb0Sea2P6G2UgxOSmgMr5ThqvINJeMloQYmbB&TARGET=-SM-https%3a%2f%2ffcamvptest%2eintra%2echrysler%2ecom%2fFGAPD%2f\\",         \\"isIframe\\": false     },     {         \\"date\\": \\"2022-07-04T16:21:17.549Z\\",         \\"timestamp\\": 1656951677549,         \\"eventType\\": \\"userNavigate\\",         \\"url\\": \\"https://login-test.chrysler.com/siteminderagent/forms/chryslerlogin.fcc?TYPE=33554433&REALMOID=06-5c594b4c-593f-10de-a09d-84fa13f20000&GUID=&SMAUTHREASON=0&METHOD=GET&SMAGENTNAME=-SM-a3lUnWazCsrbaYmfu6dxAODiSZMorSqXFnaD2MwBbbo%2bOa62sQs9JDhjaTaKb0Sea2P6G2UgxOSmgMr5ThqvINJeMloQYmbB&TARGET=-SM-https%3a%2f%2ffcamvptest%2eintra%2echrysler%2ecom%2fFGAPD%2f\\",         \\"frameId\\": 0,         \\"tabId\\": 230,         \\"windowId\\": 229     },     {         \\"date\\": \\"2022-07-04T16:21:32.588Z\\",         \\"timestamp\\": 1656951692588,         \\"windowInnerWidth\\": 1366,         \\"windowInnerHeight\\": 625,         \\"uniqueElementID\\": \\"a17vk6do78-i7eqdxtyo1k-171nolapbaxi\\",         \\"tagName\\": \\"A\\",         \\"eventType\\": \\"click\\",         \\"tagNodeIndex\\": 4,         \\"href\\": \\"#about\\",         \\"className\\": \\"btn-get-started\\",         \\"name\\": \\"\\",         \\"id\\": \\"\\",         \\"frameId\\": 0,         \\"tabId\\": 230,         \\"textContent\\": \\"Home\\",         \\"innerHTML\\": \\"Home\\",         \\"elementType\\": \\"\\",         \\"xPath\\": \\"/html/body/div[2]/section/div/div/div[2]/a\\",         \\"ariaSelector\\": \\"aria/Home\\",         \\"triggersNavigation\\": false,         \\"triggersWithinDocumentNavigation\\": true,         \\"shiftKey\\": false,         \\"ctrlKey\\": false,         \\"altKey\\": false,         \\"metaKey\\": false,         \\"windowId\\": 229,         \\"url\\": \\"https://fcamvptest.intra.chrysler.com/FGAPD/\\",         \\"isIframe\\": false     } ]","type":"RecordedLogin"}],"name":"7389_Burpent","scan_configurations":[{"name":"Audit checks - critical issues only","type":"NamedConfiguration"}],"urls":["https://fcamvptest.intra.chrysler.com/FGAPD"]}\''
         //  sh 'curl -vgw "\\n" -X POST \'http://10.172.113.186:8080/api/bThqBIr53GwDSRkFU8rezwvifYdhuUlG/v0.1/scan\' -d \'{"application_logins":[{"password":"k1ttenb0wl","type":"UsernameAndPasswordLogin","username":"t0108z0"}],"name":"valburp","scope":{"type":"SimpleScope"},"urls":["https://testvalhub.intra.chrysler.com/HelpDesk/Home"]}\''                 
           sh 'curl -vgw "\\n" -X POST \'http://10.172.99.35:8080/api/tKUIjgygImDetUbKCUmPM69zY9IliOuc/v0.1/scan\' -d \'{"application_logins":[{"password":"admin","type":"UsernameAndPasswordLogin","username":"admin"}],"name":"demotest","urls":["www.demo.testfire.net"]}\''
      } 
    }*/
       
    
 stage ('Appscan Cloud SAST') {
         steps {
      //appscan application: '4130440a-8227-4b5f-b846-e4ef704931fb', credentials: 'appscan', name: 'CICDDemoAuguest ', scanner: static_analyzer(hasOptions: false, target: '/var/lib/jenkins/workspace/CICD'), type: 'Static Analyzer'
         //  appscan application: '4130440a-8227-4b5f-b846-e4ef704931fb', credentials: 'appscancloud', name: 'CICDStatic', scanner: static_analyzer(hasOptions: false, target: '/var/lib/jenkins/workspace/CICD'), type: 'Static Analyzer'
     // appscan application: '4130440a-8227-4b5f-b846-e4ef704931fb', credentials: 'appscancloud', failBuild: true, failureConditions: [failure_condition(failureType: 'high', threshold: 1), failure_condition(failureType: 'medium', threshold: 1)], name: 'Breakthebuild', scanner: static_analyzer(hasOptions: false, target: '/var/lib/jenkins/workspace/CICD'), type: 'Static Analyzer', wait: true     
           appscan application: '4130440a-8227-4b5f-b846-e4ef704931fb', credentials: 'HCLEnt', name: 'DevSecOps', scanner: static_analyzer(hasOptions: false, target: '/var/lib/jenkins/workspace/DevSecOps/'), type: 'Static Analyzer'
      } 
    }
    
   
   stage ('Build') {
      steps {
      sh 'mvn clean package'
     }
    }


    stage ('Deploy') {
     steps {
            sh 'chmod +777 /var/lib/jenkins/workspace/DevSecOps'
            sh 'sudo cp -r /var/lib/jenkins/workspace/DevSecOps /var/www/html' 
            //sh 'chmod +777 /var/lib/jenkins/workspace/CICD/target/WebApp'
            //sh 'ls /var/www/html'
            //sh 'sudo cp -r /var/lib/jenkins/OWASP-Dependency-Check/reports /var/www/html'
       }
    }
      
  stage ('Appscan Cloud DAST') {
      steps {
      ///appscan application: '4130440a-8227-4b5f-b846-e4ef704931fb', credentials: 'appscan', name: 'CICDDemoAuguest_DynScan', scanner: dynamic_analyzer(hasOptions: false, optimization: 'Fast', scanType: 'Staging', target: 'http://altoromutual.com:8080/login.jsp'), type: 'Dynamic Analyzer'
        //appscan application: '4130440a-8227-4b5f-b846-e4ef704931fb', credentials: 'appscancloud', name: 'CICDDast', scanner: dynamic_analyzer(hasOptions: false, optimization: 'Fast', scanType: 'Staging', target: 'http://altoromutual.com:8080/login.jsp'), type: 'Dynamic Analyzer'
        appscan application: '4130440a-8227-4b5f-b846-e4ef704931fb', credentials: 'HCLEnt', name: 'DevSecOps_DAST', scanner: dynamic_analyzer(hasOptions: false, loginType: 'None', optimization: 'Fast', scanType: 'Staging', target: 'http://altoromutual.com:8080/login.jsp'), type: 'Dynamic Analyzer'
     } 
    } 
 
    
  }
} 
