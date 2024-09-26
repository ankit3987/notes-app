pipeline{
    agent any
    stages{
        stage("clone the code"){
            steps{
                echo "cloning the code"
                // sh "git clone https://github.com/ankit3987/notes-app.git"
                git url:"https://github.com/ankit3987/notes-app.git" , branch: "main"
            }
        }
        stage("build the code by docker build"){
            steps{
                echo "building the image"
                sh "docker build -t my-note-app ."
            }
        }
        stage("Push to Docker hub"){
            steps{
                echo "pushing the image to docker hub"
                withCredentials(
			[usernamePassword(
				credentialsId:"dockerHub",
				passwordVariable:"dockerHubPass",
				usernameVariable:"dockerHubUser"
				)
			]		
		){
                sh "docker tag my-note-app ${env.dockerHubUser}/my-note-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/my-note-app:latest"
                }
            }
        }
        stage("Deploy the Container"){
           steps{
                echo "Deploy the container"
                sh "docker compose down && docker compose up -d"
            } 
        }
    }
    
}
