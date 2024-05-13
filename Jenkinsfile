pipeline {
    agent any
    stages {
        stage('Clone And Pull Repositories From Git') {
            steps {
                sh '''
                    if [ -d "Backend_PD/.git" ]; then
                        cd Backend_PD
                        git pull
                        cd ..
                    else
                        git clone https://github.com/pmcoliveira98/Backend_PD.git
                    fi
                '''
                sh '''
                    if [ -d "Frontend_PD/.git" ]; then
                        cd Frontend_PD
                        git pull
                        cd ..
                    else
                        git clone https://github.com/pmcoliveira98/Frontend_PD.git
                    fi
                '''
                sh '''
                    if [ -d "Setup_PD/.git" ]; then
                        cd Setup_PD
                        git pull
                        cd ..
                    else
                        git clone https://github.com/pmcoliveira98/Setup_PD.git
                    fi
                '''
            }
        }
         stage('Login To Dockerhub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'docker_username', passwordVariable: 'docker_password')]) {
                        sh "docker login https://index.docker.io/v1/ --username \"\$docker_username\" --password \"\$docker_password\""
                    }                    
                }            
                }
        }
          stage('Build Backend') {
            steps {
                dir('Backend_PD') {
                    sh 'docker build -t pd-be -f Dockerfile .'
                    sh 'docker tag pd-be pmcoliveira98/pd-be'
                }
            }
        }
        stage('Build Frontend') {
            steps {
                dir('Frontend_PD') {
                    sh 'docker build -t pd-fe -f Dockerfile .'
                    sh 'docker tag pd-fe pmcoliveira98/pd-fe'
                }
            }
        }
        stage('Push Backend And Frontend Images') {
            steps {
                sh 'docker push pmcoliveira98/pd-be'
                sh 'docker push pmcoliveira98/pd-fe'
            }
        }




         
      

    }

   
}
