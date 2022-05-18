pipeline  {
    agent any
    tools {
        maven 'Maven'
    }
    environment{
        ArtifactId = readMavenPom().getArtifactId()
        Version = readMavenPom().getVersion()
        Name = readMavenPom().getName()
        GroupId = readMavenPom().getGroupId()
    }

   stages {
        // 1-stage
        stage('Build'){
           steps {
                sh 'mvn clean install package'
            }
        }

        stage('Unit_Test') {
          steps {
            sh 'mvn test'
          }
        }


        stage('Deploy_to_nexus') {
            steps {
               nexusArtifactUploader artifacts:
                [[
                    artifactId: "${ArtifactId}", 
                    classifier: '', 
                    file:  "target/${ArtifactId}-${Version}.war", 
                    type: 'war'
                ]], 
                   credentialsId: 'nexus-login', 
                   groupId: "${GroupId}",  
                   nexusUrl: '172.31.0.158:8081', 
                   nexusVersion: 'nexus3', 
                   protocol: 'http', 
                   repository: 'Devops_project_Snapshot', 
                   version: "${Version}"
            }
        }

        stage('Deploy_to_tomcat') {
            steps{
               echo 'deploy to tomcat.................'
            }
        }

        stage('Deploy_to_Docker') {
            steps{
               echo 'deploy to Docker.................'
            }
        }
   }
}