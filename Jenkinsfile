pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t petclinic-app .'
            }
        }

        stage('Kubernetes Deploy') {
            steps {
                sh '''
                docker save petclinic-app -o petclinic-app.tar
                sudo k3s ctr images import petclinic-app.tar
                kubectl apply -f petclinic-deployment.yaml
                kubectl apply -f petclinic-service.yaml
                '''
            }
        }
    }
}
