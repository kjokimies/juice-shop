node {
    stage('Clone Repository') {
        checkout scm
    } 
    stage('Dependency Check') {
      environment {
          analyzer.bundle.audit.enabled=false
      } 
      sh 'echo "Trying to run depedency check"'
      sh 'echo $analyzer.bundle.audit.enabled'
      dependencyCheckAnalyzer datadir: 'dependency-check-data', isFailOnErrorDisabled: true, hintsFile: '', includeCsvReports: false, includeHtmlReports: true, includeJsonReports: false, isAutoupdateDisabled: false, outdir: '', scanpath: 'package.json', skipOnScmChange: false, skipOnUpstreamChange: false, suppressionFile: '', zipExtensions: ''
      dependencyCheckPublisher canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
      archiveArtifacts allowEmptyArchive: true, artifacts: '**/dependency-check-report.*', onlyIfSuccessful: true
      step([$class: 'DependencyCheckPublisher', unstableTotalAll: '0'])
}
    stage('sonarqubeScanner') {
    // requires SonarQube Scanner 2.8+
    def scannerHome = tool 'SonarQube Scanner 3.2.0.1227';
    withSonarQubeEnv('sonarqubeServer') {
      sh "${scannerHome}/bin/sonar-scanner sonar-scanner \
         -Dsonar.projectKey=A12 \
         -Dsonar.sources=. \
         -Dsonar.host.url=http://10.48.253.181:9000 \
         -Dsonar.login=7b28fe83f6341ec39a41410ba1189192b32dfb0e"
    }
  }
}

