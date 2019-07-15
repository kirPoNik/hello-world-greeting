node('master') {

  stage('Poll') {
    checkout scm
  }
  
  stage('Build & Unit test'){
    # Where -DskipITs=true is the option to skip the integration test and perform only the build and unit test.
    sh 'mvn clean verify -DskipITs=true';
    # publish JUnit unit test reports on the Jenkins pipeline page.
    junit '**/target/surefire-reports/TEST-*.xml'
    archive 'target/*.jar'
  }
  
  stage ('Integration Test'){
    # Where -Dsurefire.skip=true is the option to skip unit testing and perform only the integration testing.
    sh 'mvn clean verify -Dsurefire.skip=true';
    # publish JUnit unit test reports on the Jenkins pipeline page.
    junit '**/target/failsafe-reports/TEST-*.xml'
    archive 'target/*.jar'
  }
  
  stage ('Publish'){
    def server = Artifactory.server 'Artifactory Server'
    def uploadSpec = """{
      "files": [
        {
          "pattern": "target/hello-0.0.1.war",
          "target": "helloworld-greeting-project/${BUILD_NUMBER}/",
          "props": "Integration-Tested=Yes;Performance-Tested=No"
        }
      ]
    }"""
    server.upload(uploadSpec)
  }

}


