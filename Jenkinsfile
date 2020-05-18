node{
     stage('SCM Checkout'){
     git 'https://github.com/hrach-hambaryan/Guacamole.git'
    }
    stage('Compile-Package'){
        // Get maven home path
        def mvnHome = tool name: 'maven', type: 'maven'
        sh "${mvnHome}/bin/mvn package"
    }
}
