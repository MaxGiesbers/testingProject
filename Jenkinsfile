node {
      stage("checkout") {
        git url: 'https://github.com/MaxGiesbers/testingProject.git'
      }

      stage("last-changes") {
        sendEmail("succes")
        getLastCommitChanges()
      }

}

 def getLastCommitChanges()
 {

   publisher = LastChanges.getLastChangesPublisher "LAST_SUCCESSFUL_BUILD", "SIDE", "LINE", true, true, "", "", "", "", ""
              publisher.publishLastChanges()
   
   return publisher
 }
 

def sendEmail(status) {
 mail (
 to: "max.giesbers@inspiro.nl", 
 subject: "Build $BUILD_NUMBER - " + status + " ($JOB_NAME)", 
 mimeType: 'text/html',
 body: """
          <p>${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}
          <br>
          Check commit changes at: <a href="${env.BUILD_URL}/last-changes">${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>
          <br>
          Check console output at: <a href=" $BUILD_URL/console">Jenkins console output</a></p>
        """)
}
