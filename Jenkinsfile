pipeline{
    
    agent {
        node{
            label 'maven'
        }
    } 
    environment {
    PATH = "/opt/apache-maven-3.9.8/bin:$PATH"
    }
        stage("build"){
            steps {
                 echo "----------- build started ----------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                 echo "----------- build complted ----------"
            }
        }
}