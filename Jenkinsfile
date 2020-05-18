    stage('SCM checkout'){
        git 'https://github.com/hrach-hambaryan/Guacamole'
        tool name: 'maven', type: 'maven'
    }	    
    stage('Compile package')
    sh 'mvn package'  
    }	    
}	
