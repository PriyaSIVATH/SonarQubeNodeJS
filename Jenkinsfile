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
        registryNexus = "192.168.0.155:8085/"
        registryNexusCredentials = "nexus-repo-manager"
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

        stage("Quality Gate Status Check"){
            steps {
                script {
                    // timeout(time: 1, unit: 'HOURS') {
                    timeout(time: 2, unit: 'MINUTES') {  
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }    
                    }  
                }   
            }
        }

        stage('Nexus Image Push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'nexus-repo-manager', url: 'http://'+ registryNexus) {
                        sh "docker  tag  ${imageName+':'+tagName}  ${registryNexus+imageName+':'+tagName}"
                        sh "docker push ${registryNexus+imageName+':'+tagName}" 
                    }
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