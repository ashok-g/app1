node('master') {
    // some block
   stage('SCM Checkout'){
     git 'https://github.com/ashok-g/app1.git'
   }
   stage('Compile-Package'){
      // Get maven home path
      def mvnHome =  tool name: 'maven-3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
   }
    
    stage('SonarQube Analysis') {
        def mvnHome =  tool name: 'maven-3', type: 'maven'
        withSonarQubeEnv('sonar-6') { 
        sh "${mvnHome}/bin/mvn sonar:sonar"
    }
        
     stage('nexus artifact uploader'){
            nexusArtifactUploader artifacts: [[artifactId: '**/*.war', classifier: '',
            file: '/var/lib/jenkins/workspace/Jenkins-Sonarqube-nexus/target/*.war', type: 'war']], 
            credentialsId: '', groupId: 'dev', nexusUrl: '', nexusVersion: 'nexus2',
            protocol: 'http', repository: '$BUILD_TIMESTAMP', version: '$BUILD_ID'

        }
}
