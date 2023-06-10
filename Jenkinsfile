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

        NEXUS_LOGIN = 'c022fcbd-7fbc-4e34-82e1-b70d360ebfa1'
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
        
         stages {
    stage('Unpack SonarScanner CLI') {
      steps {
        script {
          dir('/var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation') {
            // Assuming you have the 'unzip' command available in your Jenkins environment
            sh 'unzip -oq /path/to/sonar-scanner-cli-4.8.0.2856.zip -d sonarscanner'
          }
        }
      }
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
    }
}
