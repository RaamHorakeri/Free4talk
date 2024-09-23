
peline {
    agent any

        environment {
	        DOCKER_IMAGE = 'my-app'            // Docker image name
		        DOCKER_REPO = 'your-docker-repo'   // Replace with your Docker repo
			        K8S_NAMESPACE = 'default'          // Kubernetes namespace
				        KUBECONFIG_CRED_ID = 'kubeconfig'  // Jenkins credential ID for kubeconfig
					        REGISTRY_CREDENTIALS = 'dockerhub-credentials'  // Jenkins credential ID for DockerHub
						    }

						        stages {
							        stage('Checkout Source Code') {
								            steps {
									                    git branch: 'main', url: 'https://your-repo-url.git'
											                }
													        }

														        stage('Build Application') {
															            steps {
																                    script {
																		                        sh 'yarn install'
																					                    sh 'yarn build'
																							                    }
																									                }
																											        }

																												        stage('Build Docker Image') {
																													            steps {
																														                    script {
																																                        dockerImage = docker.build("${DOCKER_REPO}/${DOCKER_IMAGE}:${BUILD_NUMBER}")
																																			                }
																																					            }
																																						            }

																																							            stage('Push Docker Image') {
																																								                steps {
																																										                script {
																																												                    docker.withRegistry('https://index.docker.io/v1/', "${REGISTRY_CREDENTIALS}") {
																																														                            dockerImage.push()
																																																	                        }
																																																				                }
																																																						            }
																																																							            }

																																																								            stage('Run Docker Image as Container') {
																																																									                steps {
																																																											                script {
																																																													                    sh 'docker run -d -p 3000:3000 ${DOCKER_REPO}/${DOCKER_IMAGE}:${BUILD_NUMBER}'
																																																															                    }
																																																																	                }
																																																																			        }

																																																																				        stage('Deploy to Kubernetes') {
																																																																					            steps {
																																																																						                    script {
																																																																								                        withCredentials([file(credentialsId: "${KUBECONFIG_CRED_ID}", variable: 'KUBECONFIG')]) {
																																																																											                        sh """
																																																																														                        kubectl --kubeconfig=$KUBECONFIG apply -f deployment.yml
																																																																																	                        kubectl --kubeconfig=$KUBECONFIG apply -f service.yml
																																																																																				                        """
																																																																																							                    }
																																																																																									                    }
																																																																																											                }
																																																																																													        }
																																																																																														    }
																																																																																														    }
																																																																																														    pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'my-app'            // Docker image name
        DOCKER_REPO = 'your-docker-repo'   // Replace with your Docker repo
        K8S_NAMESPACE = 'default'          // Kubernetes namespace
        KUBECONFIG_CRED_ID = 'kubeconfig'  // Jenkins credential ID for kubeconfig
        REGISTRY_CREDENTIALS = 'dockerhub-credentials'  // Jenkins credential ID for DockerHub
    }

    stages {
        stage('Checkout Source Code') {
            steps {
                git branch: 'main', url: 'https://your-repo-url.git'
            }
        }

        stage('Build Application') {
            steps {
                script {
                    sh 'yarn install'
                    sh 'yarn build'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_REPO}/${DOCKER_IMAGE}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${REGISTRY_CREDENTIALS}") {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Run Docker Image as Container') {
            steps {
                script {
                    sh 'docker run -d -p 3000:3000 ${DOCKER_REPO}/${DOCKER_IMAGE}:${BUILD_NUMBER}'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withCredentials([file(credentialsId: "${KUBECONFIG_CRED_ID}", variable: 'KUBECONFIG')]) {
                        sh """
                        kubectl --kubeconfig=$KUBECONFIG apply -f deployment.yml
                        kubectl --kubeconfig=$KUBECONFIG apply -f service.yml
                        """
                    }
                }
            }
        }
    }
}
