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

    }
}
