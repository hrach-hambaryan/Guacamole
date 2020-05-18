pipeline {
    agent {
        label "master"
    }
    environment {
        //This can be nexus3 or nexus 2
        NEXUS_VERSION = "nexus3"
        //This can be http or https
        NEXUS_PROTOCOL = "http"
        // Where your nexus is running
        NEXUS_URL = "10.0.10.8:8081"
        // Repository where we will upload the artifact
        NEXUS_REPOSITORY = "Guacamole"
        // Jenkins credential id to authenticate to Nexus OSS
        NEXUS_CREDENTIAL_ID = "Nexus"
    }
    stages {
        stage("clone code") {
            steps {
                script {
                    git 'https://github.com/hrach-hambaryan/Guacamole.git'
                }
            }
        }
        stage("mvn build") {
            steps {
                script {
                    def mvnHome = tool name: 'maven', type: 'maven'
                    sh "${mvnHome}/bin/mvn -Drat.ignoreErrors=true package"
                }
            }
        }
        stage('Nexus') {
            steps {
                script {
                    def pom = readMavenPom file: 'pom.xml'
                    nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: 'Gruacamole',
                            version: "${pom.version}",
                            repository: 'nexus-guacamole',
                            credentialsId: 'Nexus',
                            artifacts: [
                                [artifactId: 'Guacamole',
                                file: '/var/jenkins_home/workspace/Guacamole/guacamole/target/*.war',
                                type: 'war']
                            ]
                    )
                }
            }
        }
    }
}
