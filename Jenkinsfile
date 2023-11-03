pipeline {
    stages {
        stage('Clone') {
            steps {
                timeout(time: 2, unit: 'MINUTES'){
                    git 'https://github.com/ytucayasi/certy-pro.git'
                }
            }
        }
        stage('Build') {
            steps {
                timeout(time: 2, unit: 'MINUTES'){
                    sh "npm install"
                }
            }
        }
        stage('Test') {
            steps {
                timeout(time: 2, unit: 'MINUTES'){
                    sh "npm test"
                }
            }
        }
        stage('Sonar') {
            steps {
                timeout(time: 2, unit: 'MINUTES'){
                  script {
                    def scannerHome = tool name: 'SonarQube Scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    def scannerScript = "${scannerHome}/bin/sonar-scanner"

                    withEnv(['PATH+SONAR_SCANNER=${scannerHome}/bin']) {
                        sh """
                            ${scannerScript}
                            -Dsonar.projectKey=pCerty
                            -Dsonar.projectName=pCerty
                            -Dsonar.sources=src
                            -Dsonar.host.url=http://sonarqube:9000
                            -Dsonar.login=a44821aa11e6c30d02a7c8593061c4659a485ee0
                        """
                    }
                  }
                }
            }
        }
        stage('Quality gate') {
            steps {

                sleep(10) //seconds

                timeout(time: 2, unit: 'MINUTES'){
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage('Deploy') {
            steps {
                echo "npm start"
            }
        }
    }
}
