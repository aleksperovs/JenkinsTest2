node {
    stage('checkout') {
        git credentialsId: '26aa5f5a-7d5f-4ab7-9af6-f0c628e39bc6', url: 'https://github.com/aleksperovs/JenkinsTest2.git'
    }
    
    stage('Compile'){
        sh 'mvn clean compile'
    }
    
    stage('Test'){
        sh 'mvn test -Dmaven.validate.skip=true -Dmaven.compile.skip=true'
    }
    
    stage('verify'){
        sh 'mvn verify -Dmaven.validate.skip=true -Dmaven.compile.skip=true -Dmaven.test.skip=true'
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
        if (env.BRANCH_NAME = 'master'){
            def scannerHome = tool 'SonarQube_Scanne01'
            withSonarQubeEnv('SonarQube') {
                sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar'
            } 
        } else {
            echo 'This is not master branch. Skipping SonarQube scan'
        }
    }
    
    stage('Archiving') {
        archiveArtifacts 'target/*.?ar'
    }
}