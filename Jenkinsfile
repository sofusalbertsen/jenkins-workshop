
node {
  cleanWs()

    stage('Preparation') { // for display purposes
        // Get some code from a GitHub repository

    checkout([$class: 'GitSCM', branches: [[name: '*/ready/**']], 
    doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'CleanBeforeCheckout'], 
    pretestedIntegration(gitIntegrationStrategy: accumulated(), integrationBranch: 'master', 
    repoName: 'origin')], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'sofusalbertsen', 
    url: 'git@github.com:sofusalbertsen/jenkins-workshop.git']]])
    
    stash name: "repo", includes: "**", excludes: "**/.git,**/.git/*", useDefaultExcludes: false

    sh 'ls'

    sh 'ls'
    }
    
    stage('Build') {
        // Run the maven build
        if (isUnix()) {
           sh 'docker run -i -u "$(id -u):$(id -g)" -v $PWD:/usr/src/mymaven -w /usr/src/mymaven --rm maven:3-jdk-8 mvn clean install'
            //sh "mvn -Dmaven.test.failure.ignore clean package"
           
        }
    }
    stage('Push'){
        pretestedIntegrationPublisher()
    when {
        branch 'master'  //only run these steps on the master branch
} steps {sh 'im on master'}
    }
    stage('Javadoc'){
          sh 'docker run -i -u "$(id -u):$(id -g)" -v $PWD:/usr/src/mymaven -w /usr/src/mymaven --rm maven:3-jdk-8 mvn clean site'
        //    archive 'target/site/*'
    }
    stage('Results') {
       
            junit '**/target/surefire-reports/TEST-*.xml'
            archiveArtifacts 'target/*.jar'
       
    }
}