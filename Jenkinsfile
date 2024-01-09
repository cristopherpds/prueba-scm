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

        stage('Exec Maven commands') {
            steps {
                dir('maven-examples/maven-example') {
                    // Configure Maven project's repositories
                    jf 'mvn-config --repo-resolve-releases libs-release --repo-resolve-snapshots libs-snapshots --repo-deploy-releases libs-release-local --repo-deploy-snapshots libs-snapshot-local'

                    // Install and publish project
                    jf 'mvn clean install'
                }
            }
        }

        stage('Publish build info') {
            steps {
                jf 'rt build-publish'
            }
        }

    }
}
