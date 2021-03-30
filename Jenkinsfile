//Trouver les variables dans environnement => localhost:8080/env-vars.html

// set CODE_CHANGES to true when code on git change
// CODE_CHANGES = getGitChanges()

//GROOVY
def gv
pipeline {

    agent any 

    tools {
        //Outil pour build l'app => Global Tool Configuration
        //maven permet les commandes mvn
        //graddle **Nom du tools paramétrer**
        //jdk et plein d'autres
    }

    environnement {
        NEW_VERSION = '1.3.0'
        SERVER_CREDENTIALS = credentials('*ID OF CREDENTIAL*')
    }

    parameters {
        //Type du parametre puis les parametres (nom, valeur, description)
        string(name: 'VERSION', defaultValue: '', description: 'version to deploy on prod')
        choice(name: 'VERSIONS', choices: ['choix1','choix2','choixN'], description: '')
        booleanParam(name: 'executeTest', defaultValue: true, description: '')
    }

    stages {

        //Etape dans Jenkins UI
        stage("build") {

            steps {
                echo "Building app"
                echo "Building version ${NEW_VERSION}"
            }
        }

        stage("test") {
            when {
                expression {
                    //excute si
                    // BRANCH_NAME == 'master' || BRANCH_NAME == 'test'
                    //CODE_CHANGES == true

                    //utilisation parameters
                    //params.executeTest 
                }
            }
            steps {
                // groovy result
                // echo "Testing app"
                // echo "${params.VERSION}"

                script {
                    gv = load "job_dsl.groovy"
                    gv.testStep()
                }
            }
        }

        stage("deploy") {

            steps {
                echo "Deploying with ${SERVER_CREDENTIALS}"
                echo "Deploying app"
                sh "${SERVER_CREDENTIALS}"
                withCredentials([
                    usernamePassword(credentials: 'ID CREDENTIALS',usernameVariable: USER, paswwordVariable: PWD)
                ]) {
                    sh "some script ${USER} ${PWD}"
                }
            }
        }
    }
    post {
        always{
            //executer peu importe l'état
        }
        success{
            //execute si état = true
        }
        failure{
            //execute si état = false
        }
    }
}