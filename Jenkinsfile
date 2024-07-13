pipeline{
    
    agent {
        node{
            label 'maven'
        }
    } 
        stages{
            stage("Clone Code"){
                steps{
                    git branch: 'main', url: 'https://github.com/sacharora999/end-to-end-pipeline.git'
                }
            }
        }
}