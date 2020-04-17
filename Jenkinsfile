pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Install Maven in container') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install && \
                    mvn -N io.takari:maven:wrapper && \
                    ./mvnw clean package && \
                    mvn archetype:generate -DgroupId=com.vogella.build.maven.java \
                    -DartifactId=com.vogella.build.maven.java  \
                    -DarchetypeArtifactId=maven-archetype-quickstart \
                    -DinteractiveMode=false'
            }
        }
        stage('Compile') {
            steps {
                sh 'mvn compile &&  \
                    mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }
}
