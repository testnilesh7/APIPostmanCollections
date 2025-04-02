pipeline {
    agent any
   
    stages {
        stage('Build') {
            steps {
                echo "Building the WAR"
                // Add your build command here
            }
        }

        stage('Deploy to QA') {
            steps {
                echo "Deploying to QA"
                // Add your deployment command here
            }
        }

        stage('Pull Docker Images') {
            parallel {
                stage('Pull GoRest Image') {
                    steps {
                        bat 'docker pull naveenkhunteta/gorestddtest:1.0'
                    }
                }
               
                stage('Pull Booking Image') {
                    steps {
                        bat 'docker pull naveenkhunteta/mybookingapi:1.0'
                    }
                }
            }
        }

        stage('Run API Test Cases in Parallel') {
            parallel {
                stage('Run GoRest Tests') {
                    steps {
                        bat 'docker run --rm -v "C:\\Users\\NILESH BHUJANG\\.jenkins\\workspace\\PostmanWithPipelineProject\\newman:/app/results" naveenkhunteta/gorestddtest:1.0'
                    }
                }
               
                stage('Run Booking Tests') {
                    steps {
                        bat 'docker run --rm -v "C:\\Users\\NILESH BHUJANG\\.jenkins\\workspace\\PostmanWithPipelineProject\\newman:/app/results" naveenkhunteta/mybookingapi:1.0'
                    }
                }
            }
        }

        stage('Publish HTML Extra Reports') {
            parallel {
                stage('Publish GoRest Report') {
                    steps {
                        publishHTML([ 
                            allowMissing: false,
                            alwaysLinkToLastBuild: false,
                            keepAll: true,
                            reportDir: 'newman',
                            reportFiles: 'gorest.html',
                            reportName: 'GoRest API Report',
                            reportTitles: ''
                        ])
                    }
                }
               
                stage('Publish Booking Report') {
                    steps {
                        publishHTML([ 
                            allowMissing: false,
                            alwaysLinkToLastBuild: false,
                            keepAll: true,
                            reportDir: 'newman',
                            reportFiles: 'booking.html',
                            reportName: 'Booking API Report',
                            reportTitles: ''
                        ])
                    }
                }
            }
        }

        stage('Deploy to PROD') {
            steps {
                echo "Deploying to PROD"
                // Add your PROD deployment command here
            }
        }
    }
}
