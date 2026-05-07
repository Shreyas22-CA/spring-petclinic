pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                sh './mvnw clean package -DskipTests -Dspring-javaformat.skip=true -Dcheckstyle.skip=true'
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
                kubectl apply --validate=false -f petclinic-deployment.yaml
		kubectl apply --validate=false -f petclinic-service.yaml
                '''
            }
        }
    }
}
