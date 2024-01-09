pipeline {
  agent any
  stages {
    stage("test"){
      steps{
      bat './gradlew test' //lancement des tests unitaires
      junit 'build/test-results/test/*.xml' //
      cucumber buildStatus: 'UNSTABLE',
      reportTitle: 'report TP',
      fileIncludePattern: 'target/report.json'
      }
    }

    stage('Code analysis') {
      steps{
       withSonarQubeEnv("sonar") { // Will pick the global server connection you have configured
         bat './gradlew sonarqube' }
      }

    }

    stage("Code Quality") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
        }

     stage("Build") {
            steps {
                bat './gradlew build'
                bat './gradlew javadoc'
                archiveArtifacts 'build/libs/*.jar'
                archiveArtifacts 'build/docs/'
            }
        }
     stage("Deploy") {
            steps {
                bat './gradlew publish'

            }
        }




}
/*
    post {
        always {
        echo "End of Pipeline process"
        mail(subject: 'End of Process Pipeline : Result incoming ...', body: 'End of Process Pipeline : Result incoming ...', from: 'ky_ghouar@esi.dz', to: 'ki_boudjadi@esi.dz')
      }
      failure {
        echo "Deployment failed"
        mail(subject: 'Deployment failed', body: 'Deployment failed ', from: 'ky_ghouar@esi.dz', to: 'ki_boudjadi@esi.dz')
      }
      success {
        echo "Deployment succeeded"
        mail(subject: 'Deployment succeeded', body: 'Deployment succeeded ', from: 'ky_ghouar@esi.dz', to: 'ki_boudjadi@esi.dz')
        notifyEvents message: 'Bonjour! : <b>Déploiement éffectué !</b> ! ', token: 'ARnvfcd-eVZwHhVHkkJlT0nTqJm8zt85'
      }
    }
*/
}
