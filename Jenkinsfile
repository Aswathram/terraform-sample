string credentialsId ='awsCredentials'

try {
stage ('checkout'){
node {
cleanWs()
checkout scm
}
}

stage('init') {
  node{
    withCredentials([[
    $class: 'AmazonWebServiceCredentialsBinding',
    credentialsId: credentialsId,
    accesskeyvariable: 'AWS_ACCESS_KEY_ID',
    secretkeyvariable: 'AWS_SECRET_ACCESS_KEY'
]]) {
  ansiColor('xterm') {
     sh 'terraform init'
  }
}
}
}

stage('plan') {
  node{
    withCredentials([[
    $class: 'AmazonWebServiceCredentialsBinding',
    credentialsId: credentialsId,
    accesskeyvariable: 'AWS_ACCESS_KEY_ID',
    secretkeyvariable: 'AWS_SECRET_ACCESS_KEY'
]]) {
  ansiColor('xterm') {
     sh 'terraform plan'
  }
}
}
}

if (env.BRANCH_NAME == 'master'){

stage('apply') {
  node{
    withCredentials([[
    $class: 'AmazonWebServiceCredentialsBinding',
    credentialsId: credentialsId,
    accesskeyvariable: 'AWS_ACCESS_KEY_ID',
    secretkeyvariable: 'AWS_SECRET_ACCESS_KEY'
]]) {
  ansiColor('xterm') {
     sh 'terraform apply'
  }
}
}
}


stage('show') {
  node{
    withCredentials([[
    $class: 'AmazonWebServiceCredentialsBinding',
    credentialsId: credentialsId,
    accesskeyvariable: 'AWS_ACCESS_KEY_ID',
    secretkeyvariable: 'AWS_SECRET_ACCESS_KEY'
]]) {
  ansiColor('xterm') {
     sh 'terraform show'
  }
}
}
}

}

currentBuild.result = 'SUCCESS'

}

catch (org.jenkinsci.plugins.workflow.steps.FlowInterruptedException flowError) {
 currentBuild.result = 'ABORTED'
}
catch (err) {
  currentBuild.result = 'FAILURE'
  throw err
}
finally {
if (currentBuild.result == 'SUCCESS'){
currentBuild.result = 'SUCCESS'
}
}
