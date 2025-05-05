pipeline{
    
    agent any;
    
    stages{
        stage("code"){
            steps{
                git url: "https://github.com/satyamkumar1507/two-tier-flask-app.git", branch: "master"
            }
        }
        stage("build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage("test"){
            steps{
                echo "code is testing"
            }
        }
        stage("push to docker hub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId: "jenkinscreds",
                    passwordVariable: "jenkinsPass",
                    usernameVariable: "jenkinsUser"
                    )]){
                sh "docker login -u ${env.jenkinsUser} -p ${env.jenkinsPass}"
                sh "docker image tag two-tier-flask-app ${env.jenkinsUser}/two-tier-flask-app"
                sh "docker push ${env.jenkinsUser}/two-tier-flask-app:latest"
                    }
            }
        }
        stage("deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
        
    }
 post{
     success{
         script{
             emailext from: 'satyamsingh150720@gmail.com',
             to: 'satyamsingh150720@gmail.com',
             body: 'Build success for demo CICD pipeline',
             subject: 'Build success for demo CICD pipeline'    
         }
     }
     failure{
         script{
             emailext from: 'satyamsingh150720@gmail.com',
             to: 'satyamsingh150720@gmail.com',
             body: 'Build failure for demo CICD pipeline',
             subject: 'Build failure for demo CICD pipeline'    
         }
     }
 }   
}
