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

        NEXUSIP = 'http://13.233.13.236:8081/'

        NEXUSPORT = '8081'

        NEXUS_GRP_REPO = 'vprof-grp'

        NEXUS_LOGIN = 'c022fcbd-7fbc-4e34-82e1-b70d360ebfa1'

       
}

    stages{
        stage('Build'){
            steps{
                 sh 'mvn -s settings.xml -DskipTests install'
            }
        }
    }
}