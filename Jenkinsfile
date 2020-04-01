pipeline {
  agent any
  tools {
      maven 'jenkins-maven'
  }
  stages {
        stage("Checkout"){
            steps {
                checkout([
                $class: 'GitSCM',
                branches: [[name: '*/master']],
                userRemoteConfigs: [[url: 'https://github.com/devopshydclub/vprofile-repo']]
                ])
            }
        }
        stage("Build"){
            steps {
            sh 'mvn install'
            }
        }
        stage("Archive Artifacts"){
            steps {
            archiveArtifacts '**/*.war'
            }
        }
        stage("Artifact upload to Nexus"){
            steps{
                nexusArtifactUploader artifacts: [[artifactId: 'vprofile-${BUILD_TIMESTAMP}',
                classifier: '',
                file: 'target/vprofile-v2.war',
                type: 'war']], credentialsId: '661db383-6240-4a0c-9dfd-b0c4ae38ac19',
                groupId: 'QA', nexusUrl: '192.168.99.10:8081',
                nexusVersion: 'nexus3',
                protocol: 'http',
                repository: 'vprofile-repo',
                version: '$BUILD_ID'
            }
        }    
    }   
}
