pipeline { 
agent any 
tools{ 
maven 'Maven-3.9'
} 
  environment { 
registry = '630006583899.dkr.ecr.us-east-1.amazonaws.com/devops_repository'
    registryCredential =  'jenkins-ecr' 
dockerimage =  ''
    } 
  stages { 
    stage( 'Checkout' ){ 
      steps{ 
        git branch:  'main' , url: 'https://github.com/MOHAMED-lab2025/-helloworld_pipeline.git'
      } 
    } 
    stage( 'Code Build' ) { 
      steps { 
        sh  'mvn clean package' 
      } 
    } 
    stage( 'Test' ) { 
      steps { 
        sh  'mvn test' 
      } 
    }
    stage( 'Build Image' ) { 
      steps { 
        script{ 
          dockerImage = docker.build registry +  ":$BUILD_NUMBER"
        } 
      } 
    } 
    stage('Docker Login') {
    steps {
        script {
            sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 630006583899.dkr.ecr.us-east-1.amazonaws.com'
        }
    }
}
    stage( 'Deploy image' ) { 
      steps{ 
        script{ 
            dockerImage.push() 
        } 
      } 
    } 
  } 
} 
