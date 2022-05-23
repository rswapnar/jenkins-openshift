! /usr/bin/env groovy

pipeline {

  agent {
    label 'maven'
  }

  stages {
    stage('Build') {
      steps {
        echo 'Building..'
        
        sh 'mvn clean package'
      }
    }
    stage('Create Container Image') {
      steps {
        echo 'Create Container Image..'
        
        script {

          
          openshift.withCluster() { 
              openshift.withProject("swapnaramesh-dev") {
  
                    def buildConfigExists = openshift.selector("bc", "jenkinsdeploy").exists() 
    
                    if(!buildConfigExists){ 
                      openshift.newBuild("--name=jenkinsdeploy", "--docker-image=registry.redhat.io/jboss-eap-7/eap74-openjdk8-openshift-rhel7", "--binary") 
                     } 
    
                    openshift.selector("bc", "jenkinsdeploy").startBuild("--from-file=target/simple-servlet-0.0.1-SNAPSHOT.war", "--follow") } }

          }
      }
    }
    stage('Deploy') {
      steps {
        echo 'Deploying....'
        script {

          
          openshift.withCluster() { 
              openshift.withProject("swapnaramesh-dev") { 
                             def deployment = openshift.selector("dc", "testjenkinsdeploy") 

                            if(!deployment.exists()){ 
                              openshift.newApp('testjenkinsdeploy', "--as-deployment-config").narrow('svc').expose() 
                            } 

                            
  } 
}

        }
      }
    }
  }
}
