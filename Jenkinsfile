pipeline {
    agent any 
    
    environment{
        fichero = fileExists file: "$ruta/miweb" 
        ruta = "/home/jenkins/workspace"
        repo = "https://github.com/JaimeRax/miweb.git"
    }

    stages {

        stage ("validación false"){
            when{expression {fichero=='false'}}
            steps{ 
                sh 'git clone $repo'
                sh 'ls -la'
            }
        }
        
        stage ("validación true"){
            when{expression {fichero=='true'}}
            steps{
                echo message: "clonando"
                dir ("$ruta/miweb){
                sh 'git pull'
                sh 'ls -la'   
                }
            }
        }

        stage ("destructiondocker"){
            steps{
            echo message: "Deteniendo contenedores"
            sh 'sudo docker stop mydb admindb web'
            echo message: "destruyendo contenedores"
            sh 'sudo docker rm mydb admindb web'
            }
        }
        
        stage ("dockerdeploy") {
            steps{
                echo message: "iniciando deploy de app"
                dir("$ruta/miweb"){
                    sh 'ls -la'
                    sh 'sudo docker-compose up -d'
                }
            }

        }     
    }
}
