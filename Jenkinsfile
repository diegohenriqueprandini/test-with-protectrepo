pipeline {

  agent any

  stages {
    stage("Protect Repo") {
      steps {
        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId:'github-diego', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
          sh '''
            set +x
            protect_repo='/usr/share/jenkins/cxfreeze-protect-repo/protect-repo'

            export TARGET_BRANCH=${CHANGE_TARGET:?}
            export GIT_COMMIT=$(git rev-parse HEAD)
            export SINCE_COMMIT=$(git merge-base "origin/$TARGET_BRANCH" "origin/$BRANCH_NAME")

            echo "$SINCE_COMMIT $GIT_COMMIT refs/origin/$BRANCH_NAME"
            echo "$SINCE_COMMIT $GIT_COMMIT refs/origin/$BRANCH_NAME" | $protect_repo \
            --report github --token $PASSWORD --pr $CHANGE_ID

            echo $(env)
          '''
        }
      }
    }
  }
}
