node('slavetwo'){
    def buildNumber = BUILD_NUMBER
    stage('git clone'){
        git credentialsId: 'gitHub_credentials', url: 'https://github.com/ganesh-solutions/httpbin.git'
    }
    stage('build Docker image'){
        sh 'docker build -t ganeshsgani/httpbin:${BUILD_NUMBER} .'
    }
    stage('push image to docker hub'){
        withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub')]) {
    sh 'docker login -u ganeshsgani -p ${dockerhub}'
        }
        sh 'docker push ganeshsgani/httpbin:${BUILD_NUMBER}'
    }
    stage('deploy application into k8s'){
        kubernetesDeploy(
            configs: 'httpbin.yml',
            kubeconfigId: 'clusterconfig',
            enableConfigSubstitution: true
            )
    }
    /*
    stage('Email Notification'){
        mail bcc: '', body: 'build completed', cc: '', from: '', replyTo: '', subject: 'from jenkins', to: 'yes.iam.ganesh.s@gmail.com'
    }
    */
}
