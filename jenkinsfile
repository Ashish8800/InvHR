pipeline{
    agent any
    
    environment {
        GITHUB_TOKEN= credentials('github-token_2')
        PATH = "$PATH:/usr/bin" // Add the directory where docker-compose is installed
    }
    
    stages{
        stage("Clone Code"){
            steps{
                echo "Cloning the code"
                git branch: 'docker', credentialsId: 'github-token_2', url: 'https://github.com/Ashish8800/InvHR'
            }
            
        }

        stage("building the frontend code"){
            steps{
                echo "Building the Image"
                sh "cd ${WORKSPACE}/frontend && docker build -t ghcr.io/pratyush-82/invihr-frontend:latest ."
            }
                       
        }   

        stage("building the backend code"){
            steps{
                echo "Building the Image"
                sh "cd ${WORKSPACE}/backend && docker build -t ghcr.io/pratyush-82/invihr-backend:latest ."
            }
                       
        }   
            
        
        stage("Push to Docker Hub"){
            steps{
                echo "Pushing the Image"
                sh "export CR_PAT=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"
                sh "echo $GITHUB_TOKEN_PSW | docker login ghcr.io -u $GITHUB_TOKEN_USR --password-stdin"
                sh "docker push ghcr.io/pratyush-82/invihr-frontend:latest"
                sh "docker push ghcr.io/pratyush-82/invihr-backend:latest"
                
            }
            
        }
        stage("Deploy"){
            steps{
                echo "Deploying the Container"
                sh "docker-compose down --rmi all && docker-compose up -d"
            }
           
        }
    }
}
