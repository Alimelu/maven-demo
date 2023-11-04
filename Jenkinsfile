pipeline {
  agent any
     tools { 
        maven 'MAVEN' 
        }
    stages{
        stage("mvn build") {
            steps {
                    sh 'mvn clean package'
         
                }
                }

        stage('sonar Analysis') {
           steps {
               sh ''' mvn sonar:sonar -Dsonar.host.url=http://3.145.150.72:9000 \
                  -Dsonar.login=squ_0800cfaf731cf33f30a78e941a21ca66ac19133e \
                  -Dsonar.java.binaries=. \
                  -Dsonar.projectkey=maven-demo '''
           }
        }
        stage("Docker") {
            steps {
                    sh 'docker build . -t mbaig2k7/myimages:1.0.4'
         
                }
                }
      
        
       stage('Docker Login') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'Docker_credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        // Use the 'withCredentials' block to securely provide credentials

                        def dockerRegistryURL = 'hub.docker.com'
                        def dockerImageName = 'myimages:1.0.4'

                        sh """
                            echo \${DOCKER_PASSWORD} | docker login --username \${DOCKER_USERNAME} --password-stdin \${dockerRegistryURL}
                            docker push docker.io/mbaig2k7/myimages:1.0.4
                        """
                    }
                }
            }
        }
}

}
