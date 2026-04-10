pipeline {
    agent {label 'maven-label'}

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven-3.9.14"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/DevopsMarch26/testrepo.git'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
        stage('upload') {
            steps {
                nexusArtifactUploader(
                  nexusVersion: 'nexus3',
                  protocol: 'http',
                  nexusUrl: 'localhost:8081',
                  credentialsId: 'nexus',
                  repository: 'maven-releases',
                  artifacts: [
                    [artifactId: 'my-app', classifier: '', file: 'target/my-app.jar', type: 'jar']
                  ]
                )

            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
    }
}

