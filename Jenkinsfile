pipeline {
    agent any
    tools {
        jdk 'JDK 12'
        maven 'Maven 3.6.2'
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
        stage ('Build web'){

        }
    }
}
