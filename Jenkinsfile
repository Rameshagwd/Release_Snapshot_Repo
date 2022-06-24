pipeline {
    agent any
    stages {
        stage ('Git Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/Rameshagwd/Release_Snapshot_Repo.git'
            } 
        }
        stage ('MVN Clean') {
            steps {
                sh 'mvn clean'
            }
        }
        stage ('MVN Compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage ('MVN Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage ('MVN Package') {
            steps {
                sh 'mvn package'
            }
        }
        stage ('Deploy .Jar/War files to Artifacts if the Snapshot or Release') {
            steps {
                script {
                    pom = readMavenPom file: 'pom.xml',
                    pom = nexusRepoName readMavenPom.version.endsWith("SNAPSHOT") ? "Snapshot_repo_nexus" : "release-repo"
                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: 'Release_Snapshot_Repo',
                            classifier: "",
                            file: "target/Release_Snapshot_Repo-${pom.version}",
                            type: jar
                        ]
                    ],

                    credentialsId: 'Nexus',
                    nexusVersion: 'nexus3',
                    nexusUrl: "10.32.39.203:8081",
                    protocol: 'http',
                    repository: nexusRepoName,
                    version: "${pom.version}"
                }
            }
        }   
    }
}