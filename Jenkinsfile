pipeline {
    agent any

    parameters {
        string(name: 'USERNAME', defaultValue: '', description: 'Podaj swój username')
        choice(name: 'BRANCH', choices: ['main'], description: '')
    }
    
    tools {
        jdk 'Java'
    }
    
    environment {
        JAVA_HOME = tool 'Java'
    }
    
    stages {
        stage('Information') {
            steps {
                echo "Hello $USERNAME, you are working on $BRANCH branch"
        stage('Checkout') {
            steps {
                git url: 'https://github.com/UB123BU/final-project.git', branch: 'main'
            }
        }
        
        stage('Compile'){
            steps {
                bat 'if not exist build\\classes mkdir build\\classes'
                bat '"%JAVA_HOME%\\bin\\javac" -d build\\classes Main.java'
            }
        }
        
        stage('Prepare Manifest') {
            steps {
                bat 'echo Main-Class: Main > build\\classes\\MANIFEST.MF'
            }
        }
        
        stage('Package') {
            steps {
                bat 'if not exist build\\jar mkdir build\\jar'
                bat 'cd build\\classes && "%JAVA_HOME%\\bin\\jar" cvmf MANIFEST.MF ..\\jar\\Final-app.jar *'
            }
        }
        
        stage('Run') {
            steps {
                bat '"%JAVA_HOME%\\bin\\java" -jar build\\jar\\Final-app.jar'
            }
        }
        stage('Pobranie Informacji o Wersji') {
            steps {
                echo "Pobrana wersja: ${env.GIT_COMMIT}"
            }
        }
    }
        post {     
            failure {       
            emailext body: "Wystąpił błąd podczas wykonywania pipelinu",                
                subject: "BŁĄD",                
                to: "kapidospamu@gmail.com"
            }
        }
}
