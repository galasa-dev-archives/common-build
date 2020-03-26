pipeline {
// Initially run on any agent
   agent { 
     label 'docker-amd64'
   }
   environment {
//Configure Maven from the maven tooling in Jenkins
      def mvnHome = tool 'Default'
      PATH = "${mvnHome}/bin:${env.PATH}"
      
//Set some defaults
      def workspace = pwd()
   }
   stages {
// Set up the workspace, clear the git directories and setup the manve settings.xml files
      stage('prep-workspace') { 
         steps {
            configFileProvider([configFile(fileId: '86dde059-684b-4300-b595-64e83c2dd217', targetLocation: 'settings.xml')]) {
            }
         
            dir('repository/dev.galasa') {
               deleteDir()
            }
            dir('repository/dev/galasa') {
               deleteDir()
            }
         }
      }
      
// Build the runtime repository
      stage('resources') {
         steps {
            withFolderProperties { 
               dir('resources') {
                  sh "mvn --settings ${workspace}/settings.xml -Dmaven.repo.local=${workspace}/repository -Dgpg.skip=true -P ${MAVEN_PROFILE} -B -e process-sources"
                  
 			      sh "docker build -t ${env.DOCKER_REPO}/galasa-allresources:${env.DOCKER_VERSION} ." 
			      sh "docker push ${env.DOCKER_REPO}/galasa-allresources:${env.DOCKER_VERSION}"                  
               }
            }
         }
      }
   }
//   post {
//       // triggered when red sign
//       failure {
//           slackSend (channel: '#galasa-devs', color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
//       }
//    }
}
