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
                    sh '''
                    pwd
                    cd Chapter08/sample1
                    chmod +x gradlew
                    ./gradlew test
                    '''
                    }

                stage("Code coverage") {
                    try {
                        sh '''
                            pwd
                            cd Chapter08/sample1
                            pwd
                            ./gradlew jacocoTestReport
                            ./gradlew jacocoTestCoverageVerification
                        '''
                    } catch (Exception E) {
                            sh '''
                            cd Chapter08/sample1
                            echo 'Failure detected!'
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
