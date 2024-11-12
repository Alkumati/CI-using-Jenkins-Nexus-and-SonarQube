pipeline {
    agent any
    tools {
        maven "MAVEN3"
        jdk "OracleJDK8"
    }
    environment {
        SNAP_REPO = "vprofile-snapshot"
        NEXUS_USER = "admin"
        NEXUS_PASS = "admin123"
        RELEASE_REPO = "vprofile-release"
        CENTRAL_REPO = "vpro-maven-central"
        NEXUS_IP = "172.31.84.141"
        NEXUS_PORT = "8081"
        NEXUS_GRP_REPO = "vpro-maven-group"
        NEXUS_LOGIN_ID = "nexuslogin"
        SONAR_SERVER = "sonarserver"  // This should match the SonarQube server configured in Jenkins
        SONAR_SCANNER = "sonarscanner"  // This should be a tool in Jenkins
    }

    stages {
        stage("BUILD THE SOURCE CODE") {
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
            post {
                success {
                    echo "NOW ARCHIVING"
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }

        stage('UNIT TEST') {
            steps {
                sh 'mvn -s settings.xml test'
            }
        }

        stage('CODE ANALYSIS WITH CHECKSTYLE') {
            steps {
                sh 'mvn -s settings.xml checkstyle:checkstyle'
            }
        }

        stage('CODE ANALYSIS with SONARQUBE') {
            environment {
                scannerHome = tool 'sonarscanner'  // Ensure 'sonarscanner' is properly configured in Jenkins
            }

            steps {
                withSonarQubeEnv("${SONAR_SERVER}") {  // Using the SONAR_SERVER variable
                    sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                        -Dsonar.projectName=vprofile \
                        -Dsonar.projectVersion=1.0 \
                        -Dsonar.sources=src/ \
                        -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                        -Dsonar.junit.reportsPath=target/surefire-reports/ \
                        -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                        -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
                }
            }
        }
    }
}
