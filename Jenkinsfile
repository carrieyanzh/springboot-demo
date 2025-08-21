pipeline {
    agent any
    tools {
        //jdk 'jdk17'
        maven 'Maven3'
    }
	 parameters {
        string(
            name: 'BRANCH_TO_BUILD',
            defaultValue: 'main',
            description: 'Enter the branch name to build'  // Parameter description
        )
        
        choice(
            name: 'ENVIRONMENT',
            choices: ['dev', 'staging', 'prod'],
            description: 'Target deployment environment'
        )
    }
    environment {
       // SCANNER_HOME=tool 'sonar-scanner'
		MAVEN_OPTS = '-Xmx1024m -Xms512m'
        BUILD_USER = "${env.BUILD_USER_ID ?: 'system'}"
    }

    stages {
		stage('Setup') {
            steps {
                script {                  
                    echo "Building branch: ${params.BRANCH_TO_BUILD}"
                    echo "Target environment: ${params.ENVIRONMENT}"
                    
                    /*
                     * TODO: Add branch validation logic here
                     * FIXME: Handle special characters in branch names
                     */
                }
            }
        }

		stage('Checkout') {
            steps {
                // Checkout the specified branch from repository
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: "*/${params.BRANCH_TO_BUILD}"]],
                    // Clone with depth 1 for faster checkout (shallow clone)
                    extensions: [[$class: 'CloneOption', depth: 1, shallow: true]],
                    userRemoteConfigs: [[url: 'https://github.com/your-org/repo.git']]
                ])
            }
        }
		
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Fir3eye/springboot-demo.git'
            }
        }
        stage('Compile') {
            steps {
                sh "mvn compile"
            }
        }
        /*stage('Test') {
            steps {
                sh "mvn test"
            }
        }*/
        /*stage('Trivy FS Scan') {
        *    steps {
        *        sh "trivy fs --format table -o fs.html . "
        *    }
        }*/
        /*stage('Build') {
            steps {
                sh "mvn package"
            }
        }*/
        /*stage("SonarQube Analysis"){
        *    steps{
        *        script{
        *            withSonarQubeEnv(credentialsId: 'sonar-token') {
        *                sh '''$SCANNER_HOME/bin/sonar-scanner \
        *                        -Dsonar.projectName=springboot \
        *                        -Dsonar.projectKey=springboot \
        *                        -Dsonar.java.binaries=target'''
        *            }
        *        }
        *    }
        *}
        *stage('Docker build and tag') {
        *    steps {
        *        sh "docker build -t fir3eye/springboot1:latest ."
        *    }
        *}
        *stage('Trivy image Scan') {
        *    steps {
        *        sh "trivy image fir3eye/springboot1:latest --format table -o image.html"
        *    }
        *}
        *stage('Docker Push Image') {
        *    steps {
        *        script{
        *            withDockerRegistry(credentialsId: 'dockerhub') {
        *                sh "docker push fir3eye/springboot1:latest"
        *          }
        *        }
        *    }
        *}
		*stage('Deploy to kubernets'){
        *    steps{
        *        script{
        *            withKubeConfig(caCertificate: '', clusterName: 'EKS_CLOUD', contextName: '', credentialsId: 'k8s', namespace: 'default', restrictKubeConfigAccess: false, serverUrl: 'https://B18E12BF81A02624B83DD385816C9EF6.gr7.ap-south-1.eks.amazonaws.com') {
        *                sh "kubectl apply -f deployment.yaml"
        *                sh "kubectl apply -f service.yaml"
        *            }
        *        }
        *    }
        *} */

    }
}
        
