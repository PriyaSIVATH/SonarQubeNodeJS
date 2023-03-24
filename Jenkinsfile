pipeline {

    agent {
        label 'docker-vm'
        // label 'docker-agent1'
    }

    environment {
        imageName = "mynodejsapp"
        tagName = "ver1.0.0"
        profileDockerHub = "priyasivath"
        scannerHome = tool "SonarScanner-Linux"
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

        stage('SonarQube Code Analysis') {
            steps {
                // One or more steps need to be included within the steps block.
                withSonarQubeEnv(installationName: 'sonarqube1') {
                    // some block
                   sh "${scannerHome}/bin/sonar-scanner" 
                }
            }
        }

        // stage('DockerHub Image Push') {
        //     steps {
        //         // One or more steps need to be included within the steps block.
        //         // This step should not normally be used in your script. Consult the inline help for details.
        //         withDockerRegistry(credentialsId: 'DockerHub-Credentials', url: '') {
        //             // some block
        //             sh "docker tag ${imageName+':'+tagName} ${profileDockerHub+'/'+imageName+':'+tagName}"
        //             sh "docker push ${profileDockerHub+'/'+imageName+':'+tagName}"
        //         }
        //     }
        // }


    }

}