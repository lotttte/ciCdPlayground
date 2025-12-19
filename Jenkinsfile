pipeline {
    agent any
    tools {
        nodejs 'yarn'
    }

    stages {
        stage('install') {
            steps {
                sh 'yarn'
            }
        }

        stage('unit_test') {
            steps {
                sh 'yarn test'
                // sh 'yarn test:e2e' **/reports/**/*.xml
            }
            post {
                always {
                    junit '**/reports/jest-junit.xml'
                }
    }

        }

        stage('build') {
            steps {
                script {
                    currentBuild.displayName = "Build Name Hier"
                    currentBuild.description = "und eine tolle Beschreibung"
                }
                sh 'yarn build'
            }
        }

        stage('e2e_test') {
            steps {
                sh 'yarn test:e2e'
            }
            post {
                always {
                    junit '**/reports/cypress-junit.xml'
                }
    }

        }

        stage('deploy') {
            steps {
                s3Upload consoleLogLevel: 'INFO', 
                  dontSetBuildResultOnFailure: false, 
                  dontWaitForConcurrentBuildCompletion: false, 
                  entries: [[
                      bucket: "cicd-workshop-playground/${env.GIT_URL.split('/')[3]}", 
                      excludedFile: '', 
                      flatten: false, 
                      gzipFiles: false, 
                      keepForever: false, 
                      managedArtifacts: false, 
                      noUploadOnFailure: false, 
                      selectedRegion: 'eu-central-1', 
                      showDirectlyInBrowser: false, 
                      sourceFile: 'public/**/*.*', 
                      storageClass: 'STANDARD', 
                      uploadFromSlave: false, 
                      useServerSideEncryption: false
                    ]], 
                    pluginFailureResultConstraint: 'FAILURE', 
                    profileName: 'role-based-access', 
                    userMetadata: []
            }
        }
    }
}
