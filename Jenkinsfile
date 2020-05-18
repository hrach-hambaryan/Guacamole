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
                    // Let's clone the source
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
         stage("publish to nexus") {
            steps {
                script {
                    pom = readMavenPom file: "pom.xml";
                    // Find build artifact under target folder
                    filesByGlob = findFiles(glob: "target/*.war");
                    // Print some info from the artifact found
                    echo "${filesByGlob}"
                }
                nexusArtifactUploader (
                    nexusVersion: NEXUS_VERSION,
                    protocol: NEXUS_PROTOCOL,
                    nexusUrl: NEXUS_URL,
                    groupId: test,
                    version: '${BUILD_NUMBER}',
                    repository: NEXUS_REPOSITORY,
                    credentialsId: NEXUS_CREDENTIAL_ID,
                    artifacts: [artifactId: pom.artifactId,
                    classifier: '',
                    file: filesByGlob,
                    type: war]
                )
            }
        } 
    }
}
