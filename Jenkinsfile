pipeline {
    agent any
    stages {
        stage("test") {
            steps {
                bat './gradlew test' //lancement des tests unitaires
                junit 'build/test-results/test/*.xml' //
                cucumber buildStatus: 'UNSTABLE',
                        reportTitle: 'report TP',
                        fileIncludePattern: 'target/report.json'
            }
        }

        stage('Code analysis') {
            steps {
                withSonarQubeEnv("sonar") {
                    bat './gradlew sonarqube'
                }
            }
        }

        stage("Code Quality") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true ie set pipeline to UNSTABLE, false = don't
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

        stage("Notification") {
            steps {
                /* script {
                   currentBuild.result = 'SUCCESS' // Reset build status to ensure notification execution
                 }*/

                echo "notificatio .."
                notifyEvents message: 'Deployment successfully', token: 'cjpgjjieyzo2mc42ejwfcmvh9etkty5a'
                /* mail(subject: 'End of Process Pipeline: Result incoming ...',
                     body: 'End of Process Pipeline: Result incoming ...',
                     from: 'ky_ghouar@esi.dz',
                     to: 'ki_boudjadi@esi.dz')*/
            }
        }
    }

    post {
        success {
            echo "Deployment succeeded"
            mail(subject: 'Deployment succeeded', body: 'Deployment succeeded ', from: 'ky_ghouar@esi.dz', to: 'ki_boudjadi@esi.dz')
           // notifyEvents message: 'Bonjour! : <b>Déploiement éffectué !</b> ! ', token: 'cjpgjjieyzo2mc42ejwfcmvh9etkty5a'
        }
        failure {
            echo "Deployment failed"
            mail(subject: 'Deployment failed', body: 'Deployment failed ', from: 'ky_ghouar@esi.dz', to: 'ki_boudjadi@esi.dz')
            notifyEvents message: 'Deployment failed', token: 'cjpgjjieyzo2mc42ejwfcmvh9etkty5a'
        }
    }
}
