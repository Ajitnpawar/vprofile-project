pipeline{
    agent any
    tools{
        maven "mvn"
        jdk "java-1.8"
    }


environment{
     SNAP_REPO = 'vropf-snap-shot'

        NEXUS_USER = 'admin'

        NEXUS_PASS = 'admin123'

        RELEASE_REPO = 'vprof-release'

        CENTRAL_REPO = 'vprof-depend-central'

        NEXUSIP = '172.31.15.28'

        NEXUSPORT = '8081'

        NEXUS_GRP_REPO = 'vprof-grp'
        NEXUS_LOGIN = 'nexuslogin1'
        SONARSERVER ='sonarserver'
        SONARSCANNER='sonarscanner'
       
}

    stages{
        stage('Build'){
            steps{
                 sh 'mvn -s settings.xml -DskipTests install'
            }
            post {
        success {
            echo "Now archiving"
            archiveArtifacts artifacts: '**/*.war'
        }
    }
        }
        stage('Test') {
            steps {
                // Step 1: Run mvn test
                sh 'mvn -s settings.xml test'
            }
        }
        stage('Static Analysis') {
            steps {
                // Step 2: Perform Checkstyle analysis
                sh 'mvn -s settings.xml checkstyle:checkstyle'
                
            }
        }
        
        
        
     stage('SonarQube Analysis') {
    environment{
        SONARSERVER ='sonarserver'
        SONARSCANNER='sonarscanner'
        scannerHome = tool "${SONARSCANNER}"
    }
            steps {
                // Step: Checkout source code from version control
                

                // Step: Set up environment variables for SonarQube
                withSonarQubeEnv("${SONARSERVER}") {
                    // Step: Run SonarQube scanner
                    sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile-repo \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
                }
            }
        }
        stage('Quality Gates') {
    steps {
        timeout(time: 1, unit: 'HOURS') {
            waitForQualityGate abortPipeline: true
        }
    }
}
        
        stage('Upload to Nexus Repository') {
    steps {
            nexusArtifactUploader (
                nexusVersion: 'nexus3',
                protocol: 'http',
                nexusUrl: '$(NEXUSIP):$(NEXUSPORT)',
                groupId: 'QA',
                version: '$(env.BUILD_ID)-$(env.BUILD_TIMESTAMP)',
                repository: '$(RELEASE_REPO)',
                credentialsId: $(NEXUS_LOGIN),
                artifacts: [
                    // List of artifacts to upload
                    [
                        artifactId: 'vproapp',
                        classifier: '',
                        file: 'target/vprofile.war',
                        type: 'war'
                    ]
                    // Add more artifacts if needed
                ]
            )
        
    }
}

        
    }
}
