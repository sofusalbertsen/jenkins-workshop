
node {
    stage('Preparation') { // for display purposes
        // Get some code from a GitHub repository

    checkout([$class: 'GitSCM', branches: [[name: '*/ready/**']], 
    doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'CleanBeforeCheckout'], 
    pretestedIntegration(gitIntegrationStrategy: accumulated(), integrationBranch: 'master', 
    repoName: 'origin')], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'sofusalbertsen', 
    url: 'git@github.com:sofusalbertsen/jenkins-workshop.git']]])
    //    git credentialsId: 'sofusalbertsen', url: 'git@github.com:sofusalbertsen/jenkins-workshop.git'
        // Get the Maven tool.
        // ** NOTE: This 'M3' Maven tool must be configured
        // **       in the global configuration.           
    }
    
    stage('Build') {
        // Run the maven build
        if (isUnix()) {
           
            sh "mvn -Dmaven.test.failure.ignore clean package"
           
        }
    }
    stage('Push'){
        pretestedIntegrationPublisher()
    
    }
    stage('Javadoc'){
           // sh "mvn site"
        //    archive 'target/site/*'
    }
    stage('Results') {
       
            junit '**/target/surefire-reports/TEST-*.xml'
            archive 'target/*.jar'
       
    }
}