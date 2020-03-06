pipeline {
    agent any
    stages {
        stage ('Clone') {
            steps {
                git branch: 'master', url: "https://github.com/shilpakallaganad19/Maven-project.git"
            }
        }

        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "Artifactory-Shilpa",
                    url: 'http://35.242.243.69:8081/artifactory/',
                    credentialsId: 'Shilpa_Artifactory_Cred1'
                )

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "Artifactory-Shilpa",
                    releaseRepo: "libs-release-local",
                    snapshotRepo: "libs-snapshot-local"
                )

                rtMavenResolver (
                    id: "MAVEN_RESOLVER",
                    serverId: "Artifactory-Shilpa",
                    releaseRepo: "libs-release",
                    snapshotRepo: "libs-snapshot"
                )
            }
        }

        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: maven, // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "MAVEN_DEPLOYER",
                    resolverId: "MAVEN_RESOLVER"
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "Artifactory-Shilpa"
                )
            }
        }
    }
}
