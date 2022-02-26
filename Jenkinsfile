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
            git branch: 'main', url: 'https://github.com/crc8109/week6'
            container('gradle') {

                stage('Build a gradle project') {
                    echo "I am the ${env.BRANCH_NAME} branch"
                    sh '''
                    pwd
                    chmod +x gradlew
                    ./gradlew test
                    '''
                    }

                stage("Code coverage") {
                    echo "CC is for Main branch; this is ${env.BRANCH_NAME}"
                    if (env.BRANCH_NAME == "main") {
                        sh '''
                            pwd
                            ./gradlew jacocoTestReport
                            ./gradlew jacocoTestCoverageVerification
                        '''


                    publishHTML (target: [
                        reportDir: 'build/reports/jacoco/test/html',
                        reportFiles: 'index.html',
                        reportName: "JaCoCo Report"
                    ])
                    }
                }

                stage("Static code analysis") {
                    echo "going to test statically now"
                    sh '''
                    pwd
                    ./gradlew checkstyleMain
                    '''
                    publishHTML (target: [
                        reportDir: 'build/reports/checkstyle/',
                        reportFiles: 'main.html',
                        reportName: "Checkstyle Report"
                    ])
                }
            }
      }
  }
}
