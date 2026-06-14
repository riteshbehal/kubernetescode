node {

    def app

    stage('Clone Repository') {
        checkout scm
    }

    stage('Build Image') {
        app = docker.build("dockercorona/test")
    }

    stage('Test Image') {
        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push Image') {
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }

    stage('Trigger Manifest Update') {
        echo "Triggering updatemanifest job"

        build job: 'updatemanifest',
              parameters: [
                  string(
                      name: 'DOCKERTAG',
                      value: env.BUILD_NUMBER
                  )
              ]
    }
}
