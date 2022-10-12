pipeline {
    agent any
    parameters {
        string(name: 'MAVEN_GOAL', defaultValue: 'clean install', description: 'maven_goal') 
    }
    triggers {
        
         pollSCM('* * * * *')
    }
                
    stages {
        stage ('vcs') {
            steps {
                git branch: "dev", url: 'git@github.com:krishna5683akp/NEWspring-petclinic.git'

            }
        }
         stage ('Artifactory configuration') {
            steps {
                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "JFROG_ID",
                    releaseRepo: "krish-libs-release-local",
                    snapshotRepo: "krish-libs-snapshot-local"
                )
            }
        }

        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: "MAVEN_DEFAULT", // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "MAVEN_DEPLOYER"
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "JFROG_ID"
                )
            }
        }
       
    }
   
}