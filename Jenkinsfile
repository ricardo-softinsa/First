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
								echo "Executing on: ${NODE_NAME}"
								checkout scm
							}
						}
	                    stage('SonarQube analysis'){
                            environment{
                                scannerHome = tool 'Scanner';
                            }
                            steps{
                                echo "SonarQube analysis"
                                withSonarQubeEnv('SonarServer') {
                                    sh "\"${scannerHome}/bin/sonar-scanner\""
                                }
                                //sleep 5
                            }
                        }
						stage('Build the Code'){
							steps{
								//bat 'gradlew assembleRelease'
								echo "Build Code"
							}
						}
					}
				}
				stage('iOS'){
					agent {
						label 'MAC_Agent'
					}
					stages{
						stage('Git Checkout'){
							steps{
								echo "Executing on: ${NODE_NAME}"
								checkout scm
							}
						}
	                    stage('SonarQube analysis'){
                            environment{
                                scannerHome = tool 'Scanner';
                            }
                            steps{
                                echo "SonarQube analysis"
                                withSonarQubeEnv('SonarServer') {
                                    sh "\"${scannerHome}/bin/sonar-scanner\""
                                }
                                //sleep 5
                            }
                        }
						stage('Build the Code'){
							steps{
								echo "Executing a build on ${NODE_NAME}"
							}
						}
					}
				}
			}
		}
	}
}
