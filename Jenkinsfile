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
        stage('Nexus') {
            steps {
                script {
                    // Read POM xml file using 'readMavenPom' step , this step 'readMavenPom' is included in: https://plugins.jenkins.io/pipeline-utility-steps
                    pom = readMavenPom file: 'pom.xml'
                    // Find built artifact under target folder
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob.name}"
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
                                file: '/var/jenkins_home/workspace/Guacamole/guacamole/target/guacamole-1.2.0.war',
                                type: 'war']
                            ]
                    )
                }
            }
        }
    }
}
