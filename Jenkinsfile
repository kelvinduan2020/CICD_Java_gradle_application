pipeline{
    agent any
    stages{
        stage("Sonar Quality Check"){
            agent{
                docker{
                    image 'openjdk:11'
                }
            }
            steps{
                script{
                    withSonarQubeEnv('sonarqube-9.2.4') {
                        sh 'chmod +x gradlew'
                        sh './gradlew clean'
                        sh './gradlew sonarqube --warning-mode=all'
                    }

                    timeout(time: 1, unit: 'HOURS') {
                      def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    }
                }
            }
        }
    }
}