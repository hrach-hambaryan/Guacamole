node {
    stage('SCM checkout'){
        git 'https://github.com/hrach-hambaryan/Guacamole'
    }
    stage('Compile package'){
    sh 'mvn package'    
    }
}
