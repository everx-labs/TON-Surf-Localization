pipeline {
    agent {label 'master'}
    tools {
        nodejs "Node14.20.0"
    }
    parameters {
        string(name: 'NEW_LOC_BETA_BRANCH', defaultValue: '', description: '(E.g: 7.0.0) New Localization branch for Surf closed beta version.')
    }
    options {
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '3')
        disableConcurrentBuilds()
    }
    stages {
        stage('Preparation') {
            failFast false
            steps {
                script {
                    timeout(time: 10, unit: 'MINUTES') {
                        def buildCause = currentBuild.getBuildCauses()
                        C_TEXT = """
                            ${buildCause.shortDescription[0]}
                            Build number: ${BUILD_NUMBER}
                            Build parameters:
                            - New Localization beta branch: **${params.NEW_LOC_BETA_BRANCH}**
                        """
                        currentBuild.description = C_TEXT;
                    }
                }
            }
        }
        stage('Creates a new Localization beta branch') {
            failFast false
            steps {
                script {
                    timeout(time: 10, unit: 'MINUTES') {
                        sh """
                        git remote update
                        git fetch --all
                        git checkout remotes/origin/development
                        """
                        sh """
                        git checkout -b surf@${params.NEW_LOC_BETA_BRANCH} origin/development
                        git push -u origin surf@${params.NEW_LOC_BETA_BRANCH}
                        """
                    }
                }
            }
        }
    }
    post {
        always {
            script {
                cleanWs notFailBuild: true
            }
        }
    }
}
