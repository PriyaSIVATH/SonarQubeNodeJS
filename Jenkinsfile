pipeline {

    agent {
        label 'docker-vm'
        // label 'docker-agent1'
    }

    environment {
        imageName = "mynodejsapp"
        tagName = "ver1.0.0"
    }

    stages {
        stage('CheckOut SCM') {
            steps {
                // One or more steps need to be included within the steps block.
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/PriyaSIVATH/SonarQubeNodeJS.git']])
            }
        }

        stage('Image Build') {
            steps {
                // One or more steps need to be included within the steps block.
                sh "docker build -t  ${imageName+':'+tagName} ."
            }
        }



    }

}