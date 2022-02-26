podTemplate(containers: [
    containerTemplate(
        name: 'gradle',
        image: 'gradle:6.3-jdk14',
        command: 'sleep',
        args: '30d'
        ),
  ]) {

    node(POD_LABEL) {
        stage('Run pipeline against a gradle project') {
            git 'https://github.com/crc8109/week6.git'
            container('gradle') {

                stage('Build a gradle project') {
                    sh '''
                    cd Chapter08/sample1
                    chmod +x gradlew
                    ./gradlew test
                    '''
                    }

                stage("Code coverage") {
                    if (env.BRANCH_NAME == "main") {
                        sh '''
                            pwd
                            cd Chapter08/sample1
                            pwd
                            ./gradlew jacocoTestReport
                            ./gradlew jacocoTestCoverageVerification
                        '''
                    }

                    publishHTML (target: [
                        reportDir: 'Chapter08/sample1/build/reports/jacoco/test/html',
                        reportFiles: 'index.html',
                        reportName: "JaCoCo Report"
                    ])
                }

                stage("Static code analysis") {
                    sh '''
                    echo "going to test statically now"
                    pwd
                    cd Chapter08/sample1
                    pwd
                    ./gradlew checkstyleMain
                    '''
                    publishHTML (target: [
                        reportDir: 'Chapter08/sample1/build/reports/checkstyle/',
                        reportFiles: 'main.html',
                        reportName: "Checkstyle Report"
                    ])
                }
            }
      }
  }
}
