pipeline {
    agent any

    environment {
        GIT_REPO_URL = 'https://github.com/Wellpass-Brasil/demo-manifest.git'
        TARGET_FILE = 'deployment.yaml'
    }

    parameters {
        string(name: 'DOCKERTAG', defaultValue: 'latest', description: 'Tag da imagem Docker que será aplicada no manifest')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Update deployment.yaml and Push') {
            steps {
                script {
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {

                            sh """
                                git config user.email "raj@cloudwithraj.com"
                                git config user.name "RajSaha"

                                echo "Antes:"
                                cat ${TARGET_FILE}

                                sed -i 's+raj80dockerid/test.*+raj80dockerid/test:${DOCKERTAG}+g' ${TARGET_FILE}

                                echo "Depois:"
                                cat ${TARGET_FILE}

                                git add ${TARGET_FILE}
                                git commit -m "Atualização via Jenkins com tag: ${DOCKERTAG}"
                                git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/demo-manifest.git HEAD:main
                            """
                        }
                    }
                }
            }
        }
    }
}
