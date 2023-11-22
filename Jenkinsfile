pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage('Git Checkout') {
            steps {
                git url: 'https://github.com/shpark1010/chinna-app.git', branch: 'main'
            }
        }
        stage('Build Maven Application') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Generate SonarQube-Analysis') {
            steps {
                withSonarQubeEnv(installationName: 'sonarqube-9', credentialsId: 'jenkins-sonar-token') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Upload War file to Nexus') {
            steps {
                script {
                    // Obtain the Jenkins build number
                    def buildNumber = currentBuild.number

                    // Use the build number in your version
                    def versionNumber = "${buildNumber}"

                    // Print the version for reference
                    echo "Version: ${versionNumber}"

                    // Upload the artifact to Nexus
                    nexusArtifactUploader artifacts:[
                        [
                            artifactId:'hiring',
                            classifier:'',
                            file: "target/hiring.war",
                            type: 'war'
                        ]
                    ],
                    credentialsId: 'nexus-credentials',
                    groupId: 'in.javahome',
                    nexusUrl: '43.201.101.185:8081',
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    repository: 'chinna-app',
                    version: versionNumber
                }
            }
        }
    }
}
