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
                git branch: 'main', url: 'https://github.com/DevopsMarch26/testrepo.git'
            }
        }
        stage('upload') {
            steps {
                sh "mvn -Dmaven.test.failure.ignore=true clean deploy"
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

