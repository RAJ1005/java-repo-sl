pipeline 
{ agent any environment 
 { 
     DOCKER_IMAGE = "raj1005/java-repo-sl" 
     DOCKER_TAG = "latest" 
 } 
 stages 
 { stage('Checkout Code') 
  { steps 
   {
      git branch: 'main', url: 'https://github.com/RAJ1005/java-repo-sl.git' 
   } 
  } 
  stage('Maven Build') 
  { 
      steps 
      { 
          sh 'mvn clean package -DskipTests' 
      }
  } 
  stage('Build Docker Image') 
  { 
      steps 
      { 
          sh 'docker build -t $DOCKER_IMAGE:$DOCKER_TAG .' 
      } 
  } 
  stage('Push to Docker Hub') 
  { 
      steps 
      { 
          withCredentials([usernamePassword(
                           credentialsId: 'dockerhub-creds', 
                           usernameVariable: 'USER', 
                           passwordVariable: 'PASS'
           )]) { 
                   sh '''
                         echo $PASS | docker login -u $USER --password-stdin
                         docker push $DOCKER_IMAGE:$DOCKER_TAG' 
                      '''
               } 
      } 
  } 
  stage('Deploy Container') 
  { 
      steps 
      { 
          sh ''' 
              docker rm -f java-app || true 
              docker run -d --name java-app -p 8080:8080 $DOCKER_IMAGE:$DOCKER_TAG 
             ''' 
      } 
  }
 } 
}
