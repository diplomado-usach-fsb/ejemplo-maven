pipeline {
    agent any

    environment {
     //Nexus verion
     NEXUS_VERSION = "nexus3"
     //Protocol
     NEXUS_PROTOCOL = "http"
     //Url
     NEXUS_URL = "localhost:8081"
     //Repository
     NEXUS_REPOSITORY = "test-nexus"
     //Jenkins Credential
     NEXUS_CREDENTIAL_ID = "test-nexus"


    }

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

        stage('sonarQube'){
            steps {
                withSonarQubeEnv(installationName: 'sonar'){
                    sh './mvnw org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
                }
            }
        }

        stage("uploadNexus") {
            steps {
                script {
                    pom = readMavenPom file: "pom.xml";
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );
                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
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
