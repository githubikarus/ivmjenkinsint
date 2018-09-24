node {
   // Define a variable to use across stages
   def my_image
   stage('Build') {
       // Get Dockerfile and code from Git (or other source control)
   checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '0579e8f0-e8a8-4d21-8004-d7a39cfe013b', url: 'https://github.com/githubikarus/ivmjenkinsint']]])
       // Build Docker image and set image reference
       my_image = docker.build("insightvm-jenkins-integration-pipeline:${env.BUILD_ID}")
       echo "Built image ${my_image.id}"
   }
   stage('Test') {
       // Assess the image
       assessContainerImage failOnPluginError: true,
           imageId: "${my_image.id}",
           thresholdRules: [
              exploitableVulnerabilities(action: 'Mark Unstable', threshold: '1')
            ],
            nameRules: [
              vulnerablePackageName(action: 'Fail', contains: 'bash')
           ]
   }
   stage('Deploy') {
       echo "Deploying image ${my_image.id} to somewhere..."
       // Push image and deploy a container
   }
}
