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

        stage('Build') { 
            steps {
                bat "mvn clean package -DskipTests=true -Dproject.versionNumber=${VERSION_NUMBER}" 
            }
        }

        stage('Push to Nexus') {
            steps {
                bat """
                    mvn deploy:deploy-file -s ci_settings_nexus.xml -DgroupID=OTPP -Dversion=${VERSION_NUMBER} -Dfile=target/petclinic.web.${VERSION_NUMBER}.war -Durl=https://nexus.octopusdemos.app/repository/TestMavinRepo -DrepositoryId=nexus-maven -DpomFile=pom.xml -DartifactId=PetClinic.Web
                """
            }
        }

        stage('Push build information') {
            steps {
                octopusPushBuildInformation \
                    toolId: 'Default', \
                    serverId: 'Octopus Deploy', \
                    spaceId: 'Spaces-1', \
                    commentParser: 'GitHub', \
                    overwriteMode: 'FailIfExists', \
                    packageId: 'PetClinic.Web', \
                    packageVersion: '${VERSION_NUMBER}', \
                    verboseLogging: false, \
                    additionalArgs: '--debug', \
                    gitUrl: 'https://github.com/twerthi/PetClinic', \
                    gitBranch: '${GIT_BRANCH}', \
                    gitCommit: '${GIT_COMMIT}'             
            }
        }
    }
}
