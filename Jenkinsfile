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
            // Genera un nombre único para el archivo deployPack.fed incorporando la fecha/tiempo o el número de compilación
            def deployPackFileName = "deployPack${BUILD_NUMBER}.fed"
            
            // Copia el archivo deployPack.fed a un nombre único
            bat "copy APIManager\\target\\deployPack.fed APIManager\\target\\${deployPackFileName}"

            // Publica el archivo con el nuevo nombre en Artifactory
            bat "jf rt u APIManager\\target\\${deployPackFileName} test-local"
            
            // Añade la propiedad de versión
            bat "jf rt sp test-local/APIManager/target/${deployPackFileName} build.number=${BUILD_NUMBER}"
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
