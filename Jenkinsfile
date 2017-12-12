def applicationId = 'example-superheroes-api'
def namespace = 'sandbox'
def buildNo = "${env.BUILD_NUMBER}"
def version = "1.0.${buildNo}" //default major version of zip artifact

properties([
  [$class: 'BuildDiscarderProperty', strategy: [$class: 'LogRotator', daysToKeepStr: '30', numToKeepStr: '10']],
  pipelineTriggers([pollSCM('H/2 * * * *')])
])

node(){
   stage 'Build API Image'
  openshiftBuild apiURL: '', authToken: '', bldCfg: applicationId, buildName: '', 
    checkForTriggeredDeployments:'false', commitID: '',
    namespace: namespace, showBuildLogs: 'false', verbose: 'false', waitTime: '',
    env : [[ name : 'JENKINS_BUILD_NO', value : buildNo ],[ name : 'APP_VERSION', value : version ]]

  stage 'Verify API Image Build'
  openshiftVerifyBuild apiURL: '', authToken: '', bldCfg: applicationId, 
    checkForTriggeredDeployments: 'false', namespace: namespace, verbose: 'false'

  stage 'Deploy API'
  openshiftDeploy apiURL: '', authToken: '', depCfg: applicationId, namespace: namespace, 
    verbose: 'false', waitTime: ''

  stage 'Verify Deployment'
  openshiftVerifyDeployment apiURL: '', authToken: '', depCfg: applicationId, namespace: namespace, 
    replicaCount: '1', verbose: 'false', verifyReplicaCount: 'false', waitTime: ''

  stage 'Tag Image'
  openshiftTag apiURL: '', authToken: '', namespace: namespace, sourceStream: applicationId, 
    sourceTag: 'latest', destinationStream: applicationId, destinationTag: version, verbose: 'false'

}