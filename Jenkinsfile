def registry = 'https://sachinfrog05.jfrog.io'
pipeline{
    
    agent {
        node{
            label 'maven'
        }
    } 
    environment {
    PATH = "/opt/apache-maven-3.9.8/bin:$PATH"
    }
    stages{
        stage("build"){
        steps {
            echo "----------- build started ----------"
            sh 'mvn clean deploy -Dmaven.test.skip=true'
            echo "----------- build complted ----------"
        }
        }



        stage("Jar Publish") {
        steps {
            script {
                    echo '<--------------- Jar Publish Started --------------->'
                     def server = Artifactory.newServer url:registry+"/artifactory" ,  credentialsId:"jfrog-creds"
                     def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}";
                     def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "jarstaging/(*)",
                              "target": "newmaven-libs-release-local/{1}",
                              "flat": "false",
                              "props" : "${properties}",
                              "exclusions": [ "*.sha1", "*.md5"]
                            }
                         ]
                     }"""
                     def buildInfo = server.upload(uploadSpec)
                     buildInfo.env.collect()
                     server.publishBuildInfo(buildInfo)
                     echo '<--------------- Jar Publish Ended --------------->'  
            
            }
        }   
    } 



    def imageName = 'sachinfrog05.jfrog.io/ui/native/docker-trial/ttrend'
   def version   = '2.1.4'
    stage(" Docker Build ") {
      steps {
        script {
           echo '<--------------- Docker Build Started --------------->'
           app = docker.build(imageName+":"+version)
           echo '<--------------- Docker Build Ends --------------->'
        }
      }
    }

            stage (" Docker Publish "){
        steps {
            script {
               echo '<--------------- Docker Publish Started --------------->'  
                docker.withRegistry(registry, 'jfrog-creds'){
                    app.push()
                }    
               echo '<--------------- Docker Publish Ended --------------->'  
            }
        }
    }





    }

}


    
    
