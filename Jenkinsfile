pipeline {
    agent any

    tools {
        jfrog 'jfrog-cli'
    }
    
    stages {
        stage('Preparation: Clone or Pull Git repo') {
            steps {
                script {
                    def folderPath = "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\${JOB_NAME}\\apim-projects"
                    def folderExists = bat script: "if exist \"${folderPath}\" (exit 0) else (exit 1)", returnStatus: true

                    if (folderExists == 0) {
                        echo "La carpeta 'apim-projects' existe."
                        dir(folderPath) {
                            bat "git pull origin main"
                        }
                    } else {
                        echo "La carpeta 'apim-projects' no existe."
                        bat "git clone -b main https://github.com/cristopherpds/apim-projects.git ${folderPath}"
                    }
                }
            }
        }

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
