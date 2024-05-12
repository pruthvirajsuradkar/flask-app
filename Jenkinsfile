pipeline {
    agent any
    
    stages {  
        stage('git clone') {
            steps {
              echo "clean workspace.."
              cleanWs()
              // git branch: 'main', url: 'https://github.com/pruthvirajsuradkar/flask-app.git'
            }
        }
        stage('docker clean-up') {
            steps {
               sh 'images=$(docker images -f dangling=true -q); if [[ ${images} ]]; then docker rmi --force ${images}; fi'
               sh '''if [ "$(docker ps -a -q -f name=flask-app)" ]; then
                    docker stop flask-app
                    docker rm flask-app
                fi'''
               sh 'docker builder prune -af'
            }
        }
        stage('docker build') {
            steps {
            sh  'docker build . -t flask-app-image'
            }
        }
        stage('docker run') {
            steps {       
                sh 'docker run -d --name flask-app -p 5000:5000 flask-app-image'
            }
        }    
    }      
}
