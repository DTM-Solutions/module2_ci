pipeline{
    agent any
    tools { mave 'maven'}
    stages{
        stage('check version') {
            steps{
                sh 'git --version'
                sh 'mvn -v'
                sh 'java -version'
            }
        }
		stage('git-clone'){
			steps{
           checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/DTM-Solutions/Etech-lab.git']]])
			}
		}
        stage('Build-Artifact-maven-build'){
            steps{
                sh "mvn clean package -DskipTests=true"
                archive 'target/*.jar'   
            }
        }
    }
}
