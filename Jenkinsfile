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
        stage('Mutation Tests - PIT') {
            steps {
                sh "mvn org.pitest:pitest-maven:mutationCoverage"
            }
            post {
                always {
                pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'
        }
      }
    }
    stage('Code-Quality'){
        steps{
            sh "mvn clean verify sonar:sonar \
                 -Dsonar.projectKey=DevOps-project \
                 -Dsonar.host.url=http://jenkinspipeline-demo.eastus.cloudapp.azure.com:9000 \
                 -Dsonar.login=6aa2cc7c3107b9b8a317e19fda07cbfd056e4db5"
        }
    }
    }
}
