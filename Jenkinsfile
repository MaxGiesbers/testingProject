// // // import jenkins.model.*
// // // jenkins = Jenkins.instance

// // node
// // {
// //   stage('SCM-checkout')
// //   {
// //     git 'https://github.com/MaxGiesbers/testingProject'
// //   }
// //   stage('Compile-Package')
// //   {
// //     echo " test"

// //     "Build $BUILD_NUMBER - " + status + " ($JOB_NAME)"
// //     "Changes:\n " + getChangeString() + "\n\n Check console output at: $BUILD_URL/console" + "\n"
// //   }
// // }



// // def getChangeString() {
// //   MAX_MSG_LEN = 100
// //   def changeString = ""
// //   echo "Gathering SCM changes"
// //   def changeLogSets = currentBuild.changeSets
// //   for (int i = 0; i < changeLogSets.size(); i++) {
// //     def entries = changeLogSets[i].items
// //       for (int j = 0; j < entries.length; j++) {aa
// //         def entry = entries[j]
// //         truncated_msg = entry.msg.take(MAX_MSG_LEN)
// //         changeString += " - ${truncated_msg} [${entry.author}]\n"
// //       }
// //     }
// //   if (!changeString) {
// //   changeString = " - No new changes"
// //   }
// //  return changeString
// // }


// node {
//       stage("checkout") {
//         git url: 'https://github.com/MaxGiesbers/testingProject.git'
//       }

//       stage("last-changes") {
//         sendEmail("succes")
//         getLastCommitChanges()
//         // getLastChanges()
//         // def publisher = LastChanges.getLastChangesPublisher "LAST_SUCCESSFUL_BUILD", "SIDE", "LINE", true, true, "", "", "", "", ""
//         //       publisher.publishLastChanges()
//         //       def changes = publisher.getLastChanges()
//         //       sendEmail("ultieme test")
//               //println(changes.getEscapedDiff())
//               // for (commit in changes.getCommits()) {
//               //     println(commit)
//               //     def commitInfo = commit.getCommitInfo()
//               //     println(commitInfo)
//               //     println(commitInfo.getCommitMessage())
//               //     println(commit.getChanges())
//               // }
//       }

// }

//  def getLastCommitChanges()
//  {

//    publisher = LastChanges.getLastChangesPublisher "LAST_SUCCESSFUL_BUILD", "SIDE", "LINE", true, true, "", "", "", "", ""
//               publisher.publishLastChanges()
   
//    return publisher
//  }
 


// // def getLastChanges()
// // {

// //   def publisher = LastChanges.getLastChangesPublisher "LAST_SUCCESSFUL_BUILD", "SIDE", "LINE", true, true, "", "", "", "", ""
// //               publisher.publishLastChanges()
// //   return publisher
// //               def changes = publisher.getLastChanges()

// // }




// def getChangeString() {
//  MAX_MSG_LEN = 100
//  def changeString = ""

//  echo "Gathering SCM changes"
//  def changeLogSets = currentBuild.changeSets
//  for (int i = 0; i < changeLogSets.size(); i++) {
//  def entries = changeLogSets[i].items
//  for (int j = 0; j < entries.length; j++) {
//  def entry = entries[j]
//  truncated_msg = entry.msg.take(MAX_MSG_LEN)
//  changeString += " - ${truncated_msg} [${entry.author}]\n"
//  }
//  }

//  if (!changeString) {
//  changeString = " - No new changes"
//  }
//  return changeString
// }

// def sendEmail(status) {
//  mail (
//  to: "max.giesbers@inspiro.nl", 
//  subject: "Build $BUILD_NUMBER - " + status + " ($JOB_NAME)", 
//  body: "See commit changes: " + getLastCommitChanges() + "\n\n Check console output at: $BUILD_URL/console" + "\n")
// }



pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/MaxGiesbers/testingProject'
            }
        }
        stage("send diff") {
            steps {
                script {
                    def publisher = LastChanges.getLastChangesPublisher null, "SIDE", "LINE", true, true, "", "", "", "", ""
                    publisher.publishLastChanges()
                    def changes = publisher.getLastChanges()
                    def diff = changes.getDiff()
                    writeFile file: 'build.diff', text: diff
                    emailext (
                      subject: "Sending diff of Job '${env.JOB_NAME} #${env.BUILD_NUMBER}'",
                      attachmentsPattern: '**/*.diff',
                      mimeType: 'text/html',
                      body: """<p>See attached diff of '${env.JOB_NAME} [${env.BUILD_NUMBER}]'.:</p>
                        <p>Check rich diff at <a href="${env.BUILD_URL}/last-changes">${env.JOB_NAME} [${env.BUILD_NUMBER}]</a></p>""",
                      to: "max.giesbers@inspiro.nl"
                    )

               }

            }
        }
    }
}