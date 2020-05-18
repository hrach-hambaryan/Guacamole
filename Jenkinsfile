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
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    // Print some info from the artifact found
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    // Extract the path from the file found
                    artifactPath = filesByGlob[0].path;
                    // Assign to  a boolean response verifying if the artifact file name exists
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}"
                    }

                }
            }
        } 
    }
}
