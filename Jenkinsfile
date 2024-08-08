pipeline {
    agent any
    tools{
        jdk 'jdk'
        maven 'maven'
    }
    environment{
        SCANNER_HOME= tool 'sonar-scanner'
    }


    stages {
        stage('git-checkout') {
@@ -21,55 +19,14 @@ pipeline {
            }
        }

        stage('Sonar Analysis') {
            steps {
               withSonarQubeEnv('sonar'){
                   sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Devops-CICD \
                   -Dsonar.java.binaries=. \
                   -Dsonar.projectKey=Devops-CICD '''
               }
            }
        }

		 stage('trivy') {
            steps {
               sh "trivy fs --security-checks vuln,config /var/lib/jenkins/workspace/CICD"
            }
        }



        stage('Code-Build') {
            steps {
               sh "mvn clean install"
            }
        }

         stage('Docker Build') {
            steps {
               script{
                   withDockerRegistry(credentialsId: '9ea0c4b0-721f-4219-be62-48a976dbeec0') {
                    sh "docker build -t  devopscicd . "
                 }
               }
            }
        }

        stage('Docker Push') {
            steps {
               script{
                   withDockerRegistry(credentialsId: '9ea0c4b0-721f-4219-be62-48a976dbeec0') {
                    sh "docker tag devopscicd adijaiswal/devopscicd:$BUILD_ID"
                    sh "docker push  adijaiswal/devopscicd:$BUILD_ID "
                 }
               }
            }
        }
		stage('K8') {
            steps {
               script{
                   kubernetesDeploy(configs: 'deploymentservice.yaml', kubeconfigId: 'kubernetes')
               }
            }
        }


    }
}
