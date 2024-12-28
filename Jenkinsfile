//@Library('jenkins-library') _

pipeline{

  agent {label 'agent'} 
    
  stages { 
	  /*stage('Checkout config files'){
      steps {
        //sh "git clean -dfx"
        sh "mkdir -p maven-conf"
        dir('maven-conf') {
            git url: 'http://172.28.128.33:7990/scm/dev/maven-conf.git', branch: 'master', credentialsId: 'df5c2417-fbe1-4e32-99e7-591f7bd89972'
        }
      }
    }*/
    stage('Build App'){
      steps {
            sh "docker run --rm -v ${workspace}:/usr/src/mymaven -v m2maven:/root/.m2:rw -w /usr/src/mymaven maven:3.5-alpine mvn clean package"
      }
        /*post {
          always {
                  junit "target/surefire-reports/*.xml"
          }
        }*/  
    }
    stage('SonarQube scan'){
      /*when {
        expression {
            return env['GIT_BRANCH'].contains('master')
        }
      }*/
      steps {
	  sh "echo sonarqube"    
        //sh "docker run --rm -v ${workspace}:/usr/src/mymaven -v ${workspace}/maven-conf:/usr/share/maven/ref/ -v m2maven:/root/.m2:rw -w /usr/src/mymaven maven:3.5-alpine mvn sonar:sonar"
      }
    }
    stage('OWASP ZAP scan'){
      steps {
        sh "docker run -t ghcr.io/zaproxy/zaproxy:stable zap-baseline.py -t http://20.6.166.53:8090/home"
      }
    }
    stage('Deploy to AKS'){
      steps {
        sh "echo deploy"
      }
    }   
    /*stage('Push to Artifactory') { 
      when {
        expression {
            return env['GIT_BRANCH'].contains('master')
        }
      }
      steps {
            script {
              def pom = readMavenPom file: 'pom.xml'
              def server = Artifactory.server('artifactory')
              def uploadSpec = """{
                  "files":[
                            {
                            "pattern": "target/*.jar",
                            "target": "apprepo/${pom.name}/${pom.version}/"
                            }
                  ]
              }"""                    
                  server.upload(uploadSpec)
                  def buildInfo = server.upload(uploadSpec)
                  server.publishBuildInfo(buildInfo)
            }
      }
    }//end push binaries
    */
  }//end stages  
  /*post {
    always {
        script {
          try {
            // Use slackNotifier.groovy from shared library and provide current build result as parameter    
            slackNotifier(currentBuild.currentResult)
            cleanWs()
          }catch (err){
            echo "Slack offline"
          }
        }  
    }
  } */   
}//end pipeline
