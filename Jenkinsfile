pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/godpop1/4task', credentialsId: 'add credentialsId'
            }
        }
        
        stage('Build') {
            steps {
                script {
                    try {
                        bat '"C:\Program Files\Microsoft Visual Studio\18\Community\MSBuild\Current\Bin\MSBuild.exe" test_repos.sln /t:Clean /p:Configuration=Release'
                    } catch (err) {
                        echo "Clean step failed, continuing with build. Error: ${err}"
                        currentBuild.result = 'UNSTABLE'
                        error("Build failed after clean step.")
                    }
            }
        }

        stage('Test') {
            steps {
                script {
                    try {
                        bat "C:\Users\godpop\Downloads\vs_mkr_test1\x64\Debug\test_repos.exe --gtest_output=xml:test_report.xml"
                    } catch (err) {
                        echo "Tests failed. Error: ${err}"
                        currentBuild.result = 'UNSTABLE'
                        error("Tests failed.")
                    }
            }
        }
    }

    post {
        always {
         cleanWs()
        }
     failure {
        echo 'Build failed!'
        }

    success {
        echo 'Build succeeded!'
        }
    }
}