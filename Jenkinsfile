    stage('SCM checkout'){
        git 'https://github.com/hrach-hambaryan/Guacamole'
    }	    
    stage('Compile package'){
    def mvnHome = tool name: 'maven', type: 'maven'
    sh "${mvnHome}"/bin/mvn package" 
    }	    
}	
