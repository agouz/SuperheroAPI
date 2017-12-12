def applicationId = 'example-superheroes-api'
def namespace = 'sandbox'
def buildNo = "${env.BUILD_NUMBER}"
def version = "1.0.${buildNo}" //default major version of zip artifact

properties([
  [$class: 'BuildDiscarderProperty', strategy: [$class: 'LogRotator', daysToKeepStr: '30', numToKeepStr: '10']],
  pipelineTriggers([pollSCM('H/2 * * * *')])
])

node('dpe-infra-slave') {

  stage 'Build API Image'
  sh """
      oc whoami
      oc project ${namespace}
      oc start-build ${applicationId} -n ${namespace}
    """
  // There is an issue kicking off builds. While start-build sometimes fails the actual build
  // pod actually launches. Hence, introducing a sleep to wait for the build pod logs to become
  // available.
  sleep 10
  sh """
      oc logs pod/${applicationId}-\$(oc get bc/${applicationId} -n ${namespace} -o jsonpath='{.status.lastVersion}')-build -f -n ${namespace}
    """

  stage 'Deploy API'
  sh """
      oc whoami
      oc project ${namespace}
      oc rollout latest dc/${applicationId} -n ${namespace}
      oc rollout status dc/${applicationId} -n ${namespace} --watch
    """

  stage 'Tag Image'
  sh """
      oc whoami
      oc project ${namespace}
      oc tag ${namespace}/${applicationId}:latest ${namespace}/${applicationId}:${version}
    """
}
