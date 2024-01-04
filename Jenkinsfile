pipeline {
    agent any
    
    tools {
        jfrog 'jfrog-cli'
    }
    
    stages {
        checkout scmGit(
    branches: [[name: 'main']],
    userRemoteConfigs: [[url: 'https://github.com/cristopherpds/prueba-scm.git']])
        
        stage('Build Packaging') {
            steps {
                dir('C:\\Axway\\axway\\apigateway\\Win32\\bin') {
                    bat '''
                    mkdir "%WORKSPACE%\\APIManager\\target"
                    setlocal enabledelayedexpansion
                    set "projectsToAdd="
                    for /D %%i in ("%WORKSPACE%\\apim-projects\\*") do (
                        set "projectsToAdd=!projectsToAdd! "%%i""
                    )
                    projpack.bat --create --passphrase-none --name deployPack --type fed --add !projectsToAdd! --projpass-none --dir "%WORKSPACE%\\APIManager\\target"
                    '''
                }
            }
        }
    }
}
