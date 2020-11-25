pipeline {
    agent any

    stages {
        stage('Compile Code') {
            steps {
                dir("/home/felipe/Documentos/Diplomado DevOps/Unidad3/TallerS4/ejemplo-maven"){
                sh './mvnw clean compile -e'
                }    
            }  
        }
        stage('Test Code') {
            steps {
                dir("/home/felipe/Documentos/Diplomado DevOps/Unidad3/TallerS4/ejemplo-maven"){
                sh './mvnw clean test -e'
                }    
            }  
        }
        stage('Jar Code') {
           steps {
                dir("/home/felipe/Documentos/Diplomado DevOps/Unidad3/TallerS4/ejemplo-maven"){
                sh './mvnw clean package -e'
                }    
            }  
        }
        stage('Run Jar') {
            steps {
                dir("/home/felipe/Documentos/Diplomado DevOps/Unidad3/TallerS4/ejemplo-maven"){
                sh 'nohup bash mvnw spring-boot:run &'
                sleep 10
                }    
            }  
        }
        stage('Testing Application') {
            steps {
                dir("/home/felipe/Documentos/Diplomado DevOps/Unidad3/TallerS4/ejemplo-maven"){
                sh 'curl -X GET http://localhost:8081/rest/mscovid/test?msg=testing'
                }    
            }  
        }
    }
}
