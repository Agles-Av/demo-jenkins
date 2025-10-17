pipeline {
  agent any

  environment {
    PATH = "${env.PATH}:/opt/homebrew/bin"
  }

  stages {
    stage('Parando los servicios') {
      steps {
        sh 'docker compose -p demo down || true'
      }
    }

    stage('Eliminando imagenes anteriores') {
      steps {
        sh '''
          IMAGES=$(docker images --filter "label=com.docker.compose.project=demo" -q)
          if [ -n "$IMAGES" ]; then
            docker rmi -f $IMAGES || true
          else
            echo "No images to remove"
          fi
        '''
      }
    }

    stage('Descargando la actualizacion del repo') {
      steps { checkout scm }
    }

    stage('Construyendo y desplegando') {
      steps {
        sh 'docker compose up -d --build'
      }
    }
  }

  post {
    success { echo 'Despliegue completado con exito' }
    failure { echo 'El despliegue ha fallado' }
    always  { echo 'Proceso de despliegue finalizado' }
  }
}
