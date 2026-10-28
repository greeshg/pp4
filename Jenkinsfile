pipeline{
    agent any

        environment{
            DOCKER_IMAGE = 'greeshma18/gree'
        }

        stages{
            stage('Clone to repository'){
                steps{
                    git branch : 'master',
                        url : 'https://github.com/greeshg/pp4.git'
                }
            }

            stage('Build docker image'){
                steps{
                    bat 'docker build -t %DOCKER_IMAGE%:v1 .'
                }
            }

            stage('Login to docker hub'){
                steps{
                    withCredentials([string(
                        credentialsID : 'dockerhub-PAT',
                        variables : 'DOCKER_PAT'
                    )]) {
                        bat '''
                        @echo off
                        echo %DOCKER_PAT% | docker login -u greeshma18 --password-stdin
                        if errorline 1 exit /b 1
                        '''
                    }
                }
            }

            stage('Push image to docker'){
                steps{
                    bat 'docker push %DOCKER_IMAGE%:v1'

                }
            }
        }

        post{
            success{
                echo 'DOcker image build and pushed successfull'
            }
            failure{
                echo 'Pipeline failed check console output'
            }
        }
}