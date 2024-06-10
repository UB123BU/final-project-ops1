pipeline {
    agent any

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
        stage('Pobranie Informacji o Wersji') {
            steps {
                echo "Pobrana wersja: ${env.GIT_COMMIT}" // Bezpośrednio użyj zmiennej GIT_COMMIT
            }
        }
    }
        post {     
            success {       
                script {         
                    def previousVersion = getPreviousVersion()         
                    if (previousVersion) {           
                        echo "Wycofywanie do poprzedniej wersji: ${previousVersion}"         
                        bat """           
                        powershell -Command "git reset --hard ${previousVersion}"           
                        powershell -Command "git push --force"           
                        """         
                    } else {           
                        echo "Nie znaleziono poprzedniej wersji do wycofania."         
                    }       
                }     
            }   
        }
    }
