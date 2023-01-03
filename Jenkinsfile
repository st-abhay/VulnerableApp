pipeline{
 agent any
 
tools{
 gradle 'Gradle'
 }
 
stages {
 stage ('Software Composition Analysis'){
 steps {
 sh 'sudo dependency-check.sh --scan . -f XML -o .'
 sh 'curl -X POST "http://192.168.6.241:8080/api/v2/import-scan/" -H "accept: application/json" -H "Authorization: Token c50c94737824e0bd561315ce8ee856849f5ba88f" -H "Content-Type: multipart/form-data" -H "X-CSRFToken: IoIe6juCf8QQxBkvAZR6aH0c0DvcZvsrcRlMvwNogRAsMfMfFBeTo9cC3yNMGdUp" -F "minimum_severity=Info" -F "active=true" -F "verified=true" -F "scan_type=Dependency Check Scan" -F "file=@dependency-check-report.xml;type=text/xml" -F "product_name=TX-DevSecOps" -F "engagement_name=DevSecOps-TX" -F "close_old_findings=false" -F "push_to_jira=false"' 
} 
}
 stage('Static Code Analysis') {
 steps {
 // SAST
 sh './gradlew sonarqube \
 -Dsonar.projectKey=TX-DevSecOps \
 -Dsonar.host.url=http://192.168.6.238:9000 \
 -Dsonar.login=b228c4c2fb84af14b5787bd3856110686539a83b'

 sh 'curl -X POST "http://192.168.6.241:8080/api/v2/import-scan/" -H "accept: application/json" -H "Authorization: Token c50c94737824e0bd561315ce8ee856849f5ba88f" -H "Content-Type: multipart/form-data" -H "X-CSRFToken: IoIe6juCf8QQxBkvAZR6aH0c0DvcZvsrcRlMvwNogRAsMfMfFBeTo9cC3yNMGdUp" -F "minimum_severity=Info" -F "active=true" -F "verified=true" -F "scan_type=SonarQube Scan" -F "file=@sonar-report-v1.1.0_java-tomcat_Sonarcloud-v8.0.0.485.html;type=text/html" -F "product_name=TX-DevSecOps" -F "engagement_name=DevSecOps-TX" -F "close_old_findings=false" -F "push_to_jira=false"' 
}
 }
 stage('Build') {
 steps {
 // for build
 sh './gradlew clean build --no-daemon' 
}
 }
 stage('Performance Testing') {
 steps{
 // for unit testing
 junit(testResults: 'build/test-results/test/*.xml', allowEmptyResults : true, skipPublishingChecks: true)
 }
 post {
 success {
 publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'build/reports/tests/test/', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
 }
 }
 }

 stage ('Staging') {
 steps {
 sshPublisher(publishers: [sshPublisherDesc(configName: 'docker-staging', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'cd /home/docker/vulnerable-staging && sudo docker images && sudo docker stop $(docker ps -a -q) && sudo docker rm $(docker ps -a -q) && sudo docker rmi $(docker images -a -q) && cd vulnerable-staging && docker-compose up -d', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: 'vulnerable-staging', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '**/*')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)]) 
}
 }


 stage ('DAST') {
 steps {
 sh './scan_start.sh'
 sh 'curl -X POST "http://192.168.6.241:8080/api/v2/import-scan/" -H "accept: application/json" -H "Authorization: Token c50c94737824e0bd561315ce8ee856849f5ba88f" -H "Content-Type: multipart/form-data" -H "X-CSRFToken: IoIe6juCf8QQxBkvAZR6aH0c0DvcZvsrcRlMvwNogRAsMfMfFBeTo9cC3yNMGdUp" -F "minimum_severity=Info" -F "active=true" -F "verified=true" -F "scan_type=Acunetix Scan" -F "file=@scan_report_dast.xml;type=text/xml" -F "product_name=TX-DevSecOps" -F "engagement_name=DevSecOps-TX" -F "close_old_findings=false" -F "push_to_jira=false"'
 }
 }
 stage ('Docker Scan') {
 steps {
 sh 'trivy image --format json -o scan_report1.json sasanlabs/owasp-vulnerableapp'

 sh 'curl -X POST "http://192.168.6.241:8080/api/v2/import-scan/" -H "accept: application/json" -H "Authorization: Token c50c94737824e0bd561315ce8ee856849f5ba88f" -H "Content-Type: multipart/form-data" -H "X-CSRFToken: IoIe6juCf8QQxBkvAZR6aH0c0DvcZvsrcRlMvwNogRAsMfMfFBeTo9cC3yNMGdUp" -F "minimum_severity=Info" -F "active=true" -F "verified=true" -F "scan_type=Trivy Scan" -F "file=@scan_report1.json;type=application/json" -F "product_name=TX-DevSecOps" -F "engagement_name=DevSecOps-TX" -F "close_old_findings=false" -F "push_to_jira=false"'

 sh 'trivy image --format json -o scan_report2.json sasanlabs/owasp-vulnerableapp-jsp'

 sh 'curl -X POST "http://192.168.6.241:8080/api/v2/import-scan/" -H "accept: application/json" -H "Authorization: Token c50c94737824e0bd561315ce8ee856849f5ba88f" -H "Content-Type: multipart/form-data" -H "X-CSRFToken: IoIe6juCf8QQxBkvAZR6aH0c0DvcZvsrcRlMvwNogRAsMfMfFBeTo9cC3yNMGdUp" -F "minimum_severity=Info" -F "active=true" -F "verified=true" -F "scan_type=Trivy Scan" -F "file=@scan_report2.json;type=application/json" -F "product_name=TX-DevSecOps" -F "engagement_name=DevSecOps-TX" -F "close_old_findings=false" -F "push_to_jira=false"'

 sh 'trivy image --format json -o scan_report3.json sasanlabs/owasp-vulnerableapp-php'

 sh 'curl -X POST "http://192.168.6.241:8080/api/v2/import-scan/" -H "accept: application/json" -H "Authorization: Token c50c94737824e0bd561315ce8ee856849f5ba88f" -H "Content-Type: multipart/form-data" -H "X-CSRFToken: IoIe6juCf8QQxBkvAZR6aH0c0DvcZvsrcRlMvwNogRAsMfMfFBeTo9cC3yNMGdUp" -F "minimum_severity=Info" -F "active=true" -F "verified=true" -F "scan_type=Trivy Scan" -F "file=@scan_report3.json;type=application/json" -F "product_name=TX-DevSecOps" -F "engagement_name=DevSecOps-TX" -F "close_old_findings=false" -F "push_to_jira=false"'\

 sh 'trivy image --format json -o scan_report4.json sasanlabs/owasp-vulnerableapp-facade'

 sh 'curl -X POST "http://192.168.6.241:8080/api/v2/import-scan/" -H "accept: application/json" -H "Authorization: Token c50c94737824e0bd561315ce8ee856849f5ba88f" -H "Content-Type: multipart/form-data" -H "X-CSRFToken: IoIe6juCf8QQxBkvAZR6aH0c0DvcZvsrcRlMvwNogRAsMfMfFBeTo9cC3yNMGdUp" -F "minimum_severity=Info" -F "active=true" -F "verified=true" -F "scan_type=Trivy Scan" -F "file=@scan_report4.json;type=application/json" -F "product_name=TX-DevSecOps" -F "engagement_name=DevSecOps-TX" -F "close_old_findings=false" -F "push_to_jira=false"'
 }
 }
 stage ('Prod-approval') {
 steps {
 input "Deploy to prod?"
 }
 }

 stage ('Production') {
 steps {
 sshPublisher(publishers: [sshPublisherDesc(configName: 'docker-production', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'docker rm $(docker ps -a -q) && docker rmi $(docker images -a -q) && cd vulnerable-prod && docker-compose up -d', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: 'vulnerable-prod', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '**/*')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)]) 
}
 } 

stage ('Infra Scan') {
 steps {
 sh 'infconfig'
 }
 }
 }
}

From <https://keep.google.com/u/0/#NOTE/1E3hSEObsoF7J43CBeEqzudjo0ND_65TUcq91og7sDSn0CNCcB0NOCgK5D7qE> 

