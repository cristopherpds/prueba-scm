pipeline {
    agent any
    tools {
        jfrog 'jfrog-cli'
    }
    
    stages {
        stage('Build Packaging') {
            steps {
                dir('C:\\Axway\\axway\\apigateway\\Win32\\bin') {
                    bat '''
                    mkdir "%WORKSPACE%\\APIManager\\target"
                    setlocal enabledelayedexpansion
                    set "projectsToAdd="
                    for /D %%i in ("%WORKSPACE%\\pr*") do (
                        set "projectsToAdd=!projectsToAdd! "%%i""
                    )
                    projpack.bat --create --passphrase-none --name deployPack --type fed --add !projectsToAdd! --projpass-none --dir "%WORKSPACE%\\APIManager\\target"
                    '''
                }
            }
        }
        stage('Publish to Artifactory') {
    steps {
        script {
            jf 'rt u APIManager\\target\\*.* test-local'
            jf 'rt bp'
            // Añadir propiedad de versión
            jf 'rt sp "test-local/APIManager/target/deployPack.fed" "build.number=%BUILD_NUMBER%"'
        }
    }
}


         /*stage('Publish to Artifactory') {
            steps {
                jf 'rt u APIManager\\target\\*.* prueba-scm'
                jf 'rt bp'

            }
        }*/

    }
}
