pipeline {
    agent any

    environment {
        PATH = '/opt/homebrew/bin:$env.PATH}'
    }

    stages {
        //Etapa para parar los servicios
        stage('Parando los servicios'){
            steps {
                sh'''
                docker-compose -p demo down || true 
                '''
            }
        }
        //Etapa para eliminar imagenes anteriores 
        stage('Eliminando imagenes anteriores'){
            steps {
                echo 'Eliminando imagenes anteriores'
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
        //Etapa para descargar la actualiacion dle repo
        stage('Descargando la actualizacion del repo'){
            steps {
                checkout scm
            }
        }
        //Etapa para construir y desplgar
        stage('Construyendo y desplegando'){
            steps {
                echo 'Construyendo y desplegando'
                sh '''
                docker-compose up -d --build
                '''
            }
        }
    }

    post {
        success{
            echo 'Despliegue completado con exito'
        }
        failure{
            echo 'El despliegue ha fallado'
        }
        always{
            echo 'Proceso de despliegue finalizado'
        }
    }
}