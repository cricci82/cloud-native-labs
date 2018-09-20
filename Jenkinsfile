pipeline {
  agent {
      label 'maven'
  }
  stages {
    stage('Build Image') {
      steps {
        script {
          openshift.withCluster() {
            def bc = openshift.startBuild("gateway-canary")
            bc.logs('-f')
          }
        }
      }
    }
    stage('Deploy Canary') {
      steps {
        script {
          openshift.withCluster() {
            def dc = openshift.selector("dc", "gateway")
            dc.rollout().latest()
            dc.rollout().status()
            sh "oc set route-backends gateway gateway=90 gateway-canary=10"
          }
        }
      }
    }
    stage('Verify Canary') {
      steps {
        script {
             promoteOrRollback = input message: 'Promote or rollback canary deployment?',
                     parameters: [choice(name: "Promote or Rollback?", choices: 'Promote\nRollback', description: '')]
        }
      }
    }
    stage("Rollback canary"){
        when{
            expression {
                return promoteOrRollback == 'Rollback'
            }
        }
        steps{
            echo "Rollback for canary deployment."
            script {
                openshift.withCluster {
                    openshift.withProject('coolstore') {
                        openshift.selector('dc', 'gateway-canary').rollout().undo()

                        //wait for rollout
                        openshift.selector('dc', 'gateway-canary').rollout().status()

                        //set canary imagestream back to production tag
                        openshift.tag("coolstore/gateway:latest", "coolstore      ay-can        st                                         }
            }
            } }
                                                                                        rom                                                                                        rom                                 enshift.withCluster() {
                openshift.withProject("coolstore") {
                    //Tag latest from build namespace
                    opensh    tag("coolstore/gatewa                    opensh    tag("coolstore/g
                    /***
                    /***
h    tag("co   h    tag("co   h    tag("co   h    tag("co   h    tag(('dh    tag("co   h    tag("co   h    tag("co   h    tag("co   h    tag(('dh    taits unh    tag("co   h    tag("co   h    tag("cish    tag("co   h    tag("co   h    tag("co   h    tag("co   h    tag(('dh    tag(      h    tag("co   h    tag("co   h    tag("co   h    tag("co   h    tag((nary=0"
                }
            }
        }
      }
    }
  }
}
