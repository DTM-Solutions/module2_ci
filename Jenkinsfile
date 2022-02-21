pipeline{
    agent any
    tools { maven 'maven'}
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
        stage('Unit Tests - JUnit and JaCoCo') {
            steps {
                sh "mvn test"
            }
            post {
              always {
                junit 'target/surefire-reports/*.xml'
                jacoco execPattern: 'target/jacoco.exec'
            }
        }
        } 
    }
}
// done
