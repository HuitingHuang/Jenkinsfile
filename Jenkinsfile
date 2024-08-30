node {
    def browser = params.BROWSER.toLowerCase()
    def specFiles = params.SPEC_FILES
    def tags = params.TAGS
    def parallelNumber = params.PARALLEL_NUMBER.toInteger()

    //Pull docker image e.g.cypress/base:latest
    docker.image('cypress/browsers:latest').inside {
        timestamps{
            stage('Checkout Cypress-Tests Code'){
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/main']],
                          userRemoteConfigs: [[url: 'https://github.com/HuitingHuang/cypress-demo.git',
                                              credentialsId: 'git-credentials-id-cony']]])
            }
            
            stage('build') {
                try{
                    echo "Running build ${env.BUILD_ID} on ${env.JENKINS_URL}"
                    sh 'npm ci'
                } catch(err){
                    echo "Caught: ${err}"
                    currentBuild.result = 'FAILURE'
                    throw err
                }
            }
            
            stage('cypress parallel tests') {
                withCredentials([string(credentialsId: 'cypress-record-key', variable: 'CYPRESS_RECORD_KEY')])  {
                    env.CYPRESS_trashAssetsBeforeRuns=false
                    env.GIT_BRANCH = 'main'
                    env.TAGS = tags
                    env.SPEC_FILES = specFiles

                    def parallelJobs = [:]
                    for (int i = 1; i <= parallelNumber; i++) {
                        def jobName = "job${i}" 
                        parallelJobs[jobName] = {
                            stage("Parallel ${jobName}:") {
                                try {
                                    echo "Running build ${env.BUILD_ID} for ${jobName}"
                                    sh "npm run test:ci:record:parallel -- --browser ${browser}"
                                    
                                } catch (Exception e) {
                                    echo "Caught: ${e}"
                                    currentBuild.result = 'FAILURE'
                                }
                            }
                        }
                    }

                    parallel parallelJobs
                }
            
            }
            
            stage("Archive"){
                try {
                    archiveArtifacts artifacts: "cypress/reports/html/index.html", allowEmptyArchive: 'true'
                    
                    publishHTML([
                        allowMissing: false, 
                        alwaysLinkToLastBuild: false, 
                        keepAll: false, 
                        reportDir: 'cypress/reports/html', 
                        reportFiles: 'index.html', 
                        reportName: 'HTML Report', 
                        reportTitles: '', 
                        useWrapperFileDirectly: true
                    ])
                } catch (err) {
                    echo "Caught: ${e}"
                    currentBuild.result = 'FAILURE'
                }
            }
            
        }
    }
    
}