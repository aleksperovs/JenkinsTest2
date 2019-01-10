node {
    stage('checkout') {
        git credentialsId: '26aa5f5a-7d5f-4ab7-9af6-f0c628e39bc6', url: 'https://github.com/aleksperovs/JenkinsTest2.git'
    }
    
    stage('compile,test, package, verify') {
        sh 'mvn clean verify'
    }
    
    stage('Archiving') {
        archiveArtifacts 'target/*.?ar'
    }
    
    
    stage('Publish test results'){
        step([$class: 'JUnitResultArchiver', testResults: 'target/surefire-reports/TEST*.xml'])
        publishHTML([allowMissing: true,
        alwaysLinkToLastBuild: false,
        keepAll: true,
        reportDir: 'target/site/jacoco/',
        reportFiles: 'index.html',
        reportName: 'Code coverage report',
        reportTitles: ''])
    }
    
    stage('Publish artifacts in Nexus'){
        sh 'mvn clean deploy -Dmaven.test.skip=true'
    }
    
    stage('SonarQube analysis'){
        def scannerHome = tool 'SonarQube_Scanne01'
        withSonarQubeEnv('SonarQube') {
            sh "${scannerHome}/bin/sonar-scanner"
        }
    }
}