def FAILED_STAGE
pipeline{
	agent none

	stages{
		stage('Pipeline'){
			parallel{
				stage('Android'){
					agent {
						label 'master'
					}
					stages{
						stage('Git Checkout'){
							steps{
								script{
                                    FAILED_STAGE=env.STAGE_NAME
                                }
								echo "Executing on: ${NODE_NAME}"
								checkout scm
							}
						}
	                    stage('SonarQube analysis'){
                            environment{
                                scannerHome = tool 'Scanner';
                            }
                            steps{
								script{
                                    FAILED_STAGE=env.STAGE_NAME
                                }
                                echo "SonarQube analysis"
                                withSonarQubeEnv('SonarServer') {
                                    sh "\"${scannerHome}/bin/sonar-scanner\""
                                }
                                //sleep 5
                            }
                        }
						stage('Build the Code'){
							steps{
								script{
                                    FAILED_STAGE=env.STAGE_NAME
                                }
								//bat 'gradlew assembleRelease'
								echo "Build Code"
							}
						}
					}
				}
				post{

				}
				stage('iOS'){
					agent {
						label 'MAC_Agent'
					}
					stages{
						stage('Git Checkout'){
							steps{
								script{
                                    FAILED_STAGE=env.STAGE_NAME
                                }
								echo "Executing on: ${NODE_NAME}"
								checkout scm
							}
						}
	                    stage('SonarQube analysis'){
                            environment{
                                scannerHome = tool 'Scanner';
                            }
                            steps{
								script{
                                    FAILED_STAGE=env.STAGE_NAME
                                }
                                echo "SonarQube analysis"
                                withSonarQubeEnv('SonarServer') {
                                    sh "\"${scannerHome}/bin/sonar-scanner\""
                                }
                                //sleep 5
                            }
                        }
						stage('Build the Code'){
							steps{
								script{
                                    FAILED_STAGE=env.STAGE_NAME
                                }
								echo "Executing a build on ${NODE_NAME}"
							}
						}
					}
				}
				post{
					success{
						slackSend color: 'good', message: "SUCCESS: ${currentBuild.fullDisplayName}\nANDROID"
					}
					failure{
						slackSend color: 'danger', message: "FAILURE: ${currentBuild.fullDisplayName}\nANDROID\nFailed On: ${FAILED_STAGE}"
					}
				}
			}
		}
	}
}
