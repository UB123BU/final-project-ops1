pipeline {
    agent any

    parameters {
        string(name: 'Podaj wzrost w cm:', defaultValue: '', description: 'wzrost')
        string(name: 'Podaj wagę w kg:', defaultValue: '', description: 'waga')
    }
    
    tools {
        jdk 'Java'
    }
    
    environment {
        JAVA_HOME = tool 'Java'
    }
    
    stages {
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
                bat 'cd build\\classes && "%JAVA_HOME%\\bin\\jar" cvmf MANIFEST.MF ..\\jar\\MyApplication.jar *'
            }
        }
        
        stage('Run') {
            steps {
                bat '"%JAVA_HOME%\\bin\\java" -jar build\\jar\\MyApplication.jar'
            }
        }
    }
}
