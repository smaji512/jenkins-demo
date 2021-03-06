pipeline {

    agent any
    parameters {

		string(name: 'email',

		defaultValue: 'subrath.maji@gmail',

		description: 'recieptient email')

	} 

    stages {

        stage('Clone') {

            steps {

                echo "Checkout the source code from git"

                git 'https://github.com/smaji512/jenkins-demo.git'

            

            }

        }

        stage('build'){

            steps {

                echo "building the project"

                sh "cd MavenProject ; mvn clean install ; pwd"

            }

        }

        

        stage('Archieve Artifacts'){

            steps {

                echo "archiving the artifacts"

                archiveArtifacts 'MavenProject/multi3/target/*.war'

            }

            

        }

        stage('Deployment') {

            // Deployment

            steps {

                script {

                    echo "deployment"

                    sh 'cp MavenProject/multi3/target/*.war /opt/tomcat/apache-tomcat-8.5.50/webapps/'

                }

            }

        }

        stage('publish html report') {

            steps{

                echo "publishing the html report"

                publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '/home/ubuntu/jenkins-demo/', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: ''])

            }

        }

        

        stage("Metrics"){

            steps{

            parallel ( "JavaNcss Report": {

                sh "cd javancss-master ; mvn test javancss:report ; pwd"

                  

				},

            "FindBugs Report" : {

                sh "mkdir javancss1 ; cd javancss1 ;pwd"

                sh "cd javancss-master ; mvn findbugs:findbugs ; pwd"

              



				},

			"Cobertura Report" : {

				echo "Cobertura Report generation"

                sh "cd MavenProject ; mvn cobertura:cobertura ; pwd"

				}

         )

            }

         post{

                success {

                    emailext body: 'Successfully completed pipeline project with archiving the artifacts', subject: 'Pipeline was successfull', to: "${params.email}"

                }

			}

		}

		stage('clean up') {

            steps {

                echo "cleaning up the workspace"

                cleanWs()

            }

        }

        

    }

}


