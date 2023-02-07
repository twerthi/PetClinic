pipeline {
    agent any
    tools {
        jdk 'JDK 12'
    }
    stages {
        stage ('Set version') {
            steps {
                script {
                    TEST_VAR = new Date().format("yyyy.MM.dd") as String
                    echo "${TEST_VAR}"
                    VERSION_NUMBER = VersionNumber(versionNumberString: "${TEST_VAR}.${BUILD_ID}")
                    currentBuild.displayName = "${VERSION_NUMBER}"
                }
            }
        }

        stage ('Build Web') {
            withMaven(
                maven: 'maven-3'
            ) {
                sh "mvn clean package -DskipTests=true -Dproject.versionNumber=${VERSION_NUMBER}"
            }
        }
    }
}
