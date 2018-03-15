pipeline {
  agent any
  options{
  	timestamps()
	gitLabConnection('')

  		 }

	parameters{
		string(defaultValue: 'dap-develop', description: '', name: 'ARCH_BRANCH')
		string(defaultValue: 'dap-develop', description: '', name: 'APPCORE_BRANCH')
		string(defaultValue: 'dap-develop', description: '', name: 'UIKIT_BRANCH')
		string(defaultValue: 'dap-develop', description: '', name: 'SDK_BRANCH')}


  stages {

  	stage('SCM') {

		steps{
  		checkout([$class: 'GitSCM',
			  branches: [[name: 'dap-develop']],
			  doGenerateSubmoduleConfigurations: false,
			  extensions: [[$class: 'SubmoduleOption', disableSubmodules: false, parentCredentials: true, recursiveSubmodules: true, reference: '', timeout: 15, trackingSubmodules: true]],
			  submoduleCfg: [],
			  userRemoteConfigs: [[credentialsId: 'c39b0118-cfc4-4024-ac99-cdf2f69ed733', url: 'ssh://git@coderepository.mcd.com:8443/gma5_us/android.git']]])
			}
  		}

        stage('Checkout') {
          steps {
            sh '''
	    MCDARCH_DIR=${WORKSPACE}
MCDAPPCORE_DIR="${MCDARCH_DIR}/libraries/android-mcd-core-app"
MCDUIKIT_DIR="${MCDARCH_DIR}/libraries/android-mcd-core-app/libraries/android-mcd-uikit"
MCDCONNECT_DIR="${MCDARCH_DIR}/libraries/android-mcd-core-app/libraries/android-gma-sdk-sapient"
			echo "*************Checking out McDAppARCH Repository*****************"
cd "${MCDARCH_DIR}"
git config -f .gitmodules submodule.libraries/android-mcd-core-app.branch beta/DAP-6416
git checkout beta/DAP-6416
git branch --set-upstream-to=origin/beta/DAP-6416 beta/DAP-6416
git pull
git config -f .gitmodules submodule.libraries/android-mcd-core-app.branch beta/DAP-6416
git submodule
git submodule init
git submodule sync
git config --get remote.origin.url
git submodule update --init --remote --recursive
git branch
#git submodule update --init --recursive libraries/android-mcd-core-app
git log --pretty=format:'%h' -n 1

echo "*************Checking out McDAppCore Repository*****************"
cd "${MCDARCH_DIR}"
cd "${MCDAPPCORE_DIR}"
git checkout beta/DAP-6416
git branch --set-upstream-to=origin/beta/DAP-6416 beta/DAP-6416
git pull
git config -f .gitmodules submodule.libraries/android-mcd-uikit.branch beta/DAP-6416
git config -f .gitmodules submodule.libraries/android-gma-sdk-sapient.branch beta/DAP-6416
#git submodule update --remote --recursive
git submodule
git submodule init
git submodule sync
git config --get remote.origin.url
git submodule update --init --remote --recursive
git branch
git branch
git reset --hard
#git pull
#git submodule update --checkout --recursive
git checkout beta/DAP-6416
git log --pretty=format:'%h' -n 1

#echo "*************Checking out McDonaldsSDK Repository*****************"
cd "${MCDARCH_DIR}"
git config -f .gitmodules submodule.libraries/android-gma-sdk-sapient.branch beta/DAP-6416
cd "${MCDCONNECT_DIR}"
git gc --prune=now
git checkout beta/DAP-6416
git pull
git branch
git reset --hard
#git pull
#git submodule update --checkout --recursive
#git checkout ${ARCH_BRANCH}
#git log --pretty=format:'%h' -n 1

#echo "*************Checking out McDUIKit Repository*****************"
cd "${MCDARCH_DIR}"
cd "${MCDUIKIT_DIR}"
git gc --prune=now
git checkout beta/DAP-6416
git pull
git branch
git reset --hard
#git pull
#git submodule update --checkout --recursive
#git checkout ${ARCH_BRANCH}
#git log --pretty=format:'%h' -n 1'''

sh '''
#echo "*******************Checking if date is empty then set to 1 day limit***********"
#if [ -z "${START_DATE}" ] || [ -z "${END_DATE}" ]; then
#	START_DATE="$(date -v -1d +'%b %d %Y')"
#	END_DATE="$(date +'%b %d %Y')"
#	echo "Start Date: ""$START_DATE"
#    echo "End Date: ""$END_DATE"
#fi


RELEASE_NOTES1=""
NEWLINE=$'\n'
echo "BUILD_TIMESTAMP"=`date +'%Y-%m-%d %H:%M:%S:%Z'` > timestamp.txt
echo "*******************Updating submodule*************************"
MCDARCH_DIR="${WORKSPACE}"
MCDAPPCORE_DIR="${MCDARCH_DIR}/libraries/android-mcd-core-app"
MCDUIKIT_DIR="${MCDARCH_DIR}/libraries/android-mcd-core-app/libraries/android-mcd-uikit"
MCDCONNECT_DIR="${MCDARCH_DIR}/libraries/android-mcd-core-app/libraries/android-gma-sdk-sapient"

echo "*************Checking out McDAppARCH Repository*****************"

cd "${MCDARCH_DIR}"
git checkout "${ARCH_BRANCH}"
git pull
#RELEASE_NOTES1="$RELEASE_NOTES1""$NEWLINE""APP commit id""$NEWLINE"
RELEASE_NOTES1=$RELEASE_NOTES1$(git log --no-merges '--pretty=format:%h : %s %cD' --since="$START_DATE" --until="$END_DATE")

echo "*************Checking out McDAppCore Repository*****************"

cd "${MCDAPPCORE_DIR}"
git checkout "${APPCORE_BRANCH}"
git pull
#RELEASE_NOTES1="$RELEASE_NOTES1""$NEWLINE""Core Layer commit id""$NEWLINE"
RELEASE_NOTES1=$RELEASE_NOTES1$(git log --no-merges '--pretty=format:%h : %s %cD' --since="$START_DATE" --until="$END_DATE")

echo "*************Checking out McDUIKit Repository*****************"

cd "${MCDUIKIT_DIR}"
git checkout "${UIKIT_BRANCH}"
git pull
#RELEASE_NOTES1="$RELEASE_NOTES1""$NEWLINE""UI Kit commit id""$NEWLINE"
RELEASE_NOTES1=$RELEASE_NOTES1$(git log --no-merges '--pretty=format:%h : %s %cD' --since="$START_DATE" --until="$END_DATE")

echo "*************Checking out McDonaldsSDK Repository*****************"

cd "${MCDCONNECT_DIR}"
git checkout "${SDK_BRANCH}"
git pull
#RELEASE_NOTES1="$RELEASE_NOTES1""$NEWLINE""SDK Layer commit id""$NEWLINE"
RELEASE_NOTES1=$RELEASE_NOTES1$(git log --no-merges '--pretty=format:%h : %s %cD' --since="$START_DATE" --until="$END_DATE")

cd $WORKSPACE
echo "$RELEASE_NOTES1"
#echo `cat timestamp.txt` > release_notes.txt
echo "$NEWLINE""$RELEASE_NOTES" >> release_notes.txt
echo "$NEWLINE""$RELEASE_NOTES1" >> release_notes.txt'''
sh ''' MCDAPP_DIR=${WORKSPACE}
cd ..
cp -f /android/uskeystore/release.keystore .
cd "${MCDAPP_DIR}"
cp -f /android/uskeystore/keystore.properties .'''


          }
        }


   stage('Build') {
      steps {
        sh '''
	  #run build
	  ./gradlew clean assembleUSRelease
	 '''
    }
    }

/*tage('Test') {
      steps {
        sh './gradlew cleanTest test jacocoTestReport --continue --info'
      }
    } */


    stage('Hockeyapp') {
    	steps {
    	step([$class: 'HockeyappRecorder',
			applications: [[apiToken: '41f5cf2e56fe48d2bd099f98a4011184',
      downloadAllowed: false,
			filePath: 'app/build/outputs/apk/app-US-release.apk',
			mandatory: false, notifyTeam: true,
			releaseNotesMethod: [$class: 'NoReleaseNotes'],
			uploadMethod: [$class: 'VersionCreation',
			appId: '047bbc5ea849465886898cdd07fde141']]],
			debugMode: false,
			failGracefully: false])

    	 	 }
}
    }
 post {

		success {

			emailext (
          		subject: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
          		body: """<p>SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
          		  <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
         		recipientProviders: [[$class: 'DevelopersRecipientProvider']],
            to: 'rushikesh.jawali@capgemini.com'
				)

		}


	        failure {

			emailext (
          		subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
          		body: """<p>FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
          		  <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
          		recipientProviders: [[$class: 'DevelopersRecipientProvider']],
              to: 'rushikesh.jawali@capgemini.com'
       			 	)

        }
    }
}
