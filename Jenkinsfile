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
        stage('Version') {
            steps {
                echo "Running ${env.BUILD_ID}"
            }
        }
        stage('Send Email Notifications') {
            steps {
                emailext(
                    body: '''
                        Job '${currentBuild.fullDisplayName}' (${currentBuild.number}) completed with status: ${currentBuild.result}
 
                        Console output: ${jobUrl}console
                    ''',
                    subject: '[Jenkins] Job Status Notification: ${currentBuild.fullDisplayName} (#${currentBuild.number})',
                    recipientRecipients: 'k.kapitula.063@studms.ug.edu.pl',
                    replyTo: 'kapidospamu@gmail.com',
                    attachBuildLog: true, // Attach build logs to the email (optional)
                    from: 'kapidospamu@gmail.com'
                )
    }
}
    }}
