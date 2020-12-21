
pipeline { 
    environment { 
        registry = "chamathka202602/one" 
        registryCredential = 'dockerhub_1' 
        dockerImage = '' 
    }
    agent any 
    stages { 
        stage('Cloning our Git') { 
            steps { 
                //sh "git clone https://github.com/Chamathka-Rush/anchore-test.git"
                echo "cloned the repository"
            }
        } 
        stage('Building our image') { 
            steps { 
                script { 
                    dockerImage = docker.build("${registry}:$BUILD_NUMBER") 
                    echo "${dockerImage}"
                }
            } 
        }
        stage('Docker Push') { 
            steps { 
                script { 
                    docker.withRegistry( '', registryCredential ) {                         
                    dockerImage.push() 
                    // sh "docker push $registry:$BUILD_NUMBER"    
                    }
                } 
            }
        }
        
        stage('Anchore analysis'){
            steps{
                script{
                    sh 'echo "docker.io/${registry}:$BUILD_NUMBER ${WORKSPACE}/Dockerfile" > anchore_images'
                    anchore forceAnalyze: true, bailOnFail: false, timeout: -1.0, name: 'anchore_images'
                    sh "docker rmi $registry:$BUILD_NUMBER"
                }
            }
        }
         
    }
}
