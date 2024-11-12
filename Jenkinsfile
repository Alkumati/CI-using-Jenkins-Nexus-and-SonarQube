pipeline{
    agent any
    tools{
        maven "MAVEN3"
        jdk "OracleJDK8"
    }
    environment{
        SNAP_REPO = "vprofile-snapshot"
        NEXUS_USER = "admin"
        NEXUS_PASS = "admin123"
        RELEASE_REPO = "vprofile-release"
        CENTRAL_REPO = "vpro-maven-central"
        NEXUS_IP = "172.31.84.141"
        NEXUS_PORT = "8081"
        NEXUS_GRP_REPO = "vpro-maven-group"
        NEXUS_LOGIN_ID = "nexuslogin"
    }

    stages{
        stage("BUILD THE SOURCE CODE"){
            steps{
                sh 'mvn -s setting.xml -DskipTests install'
            }
        }
    }
}