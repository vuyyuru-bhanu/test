pipeline {
    agent { label 'mav' }

    tools {
        jdk 'jdk8' // Use default JDK for initial stages
    }

    stages {
        stage('Checkout Game of Life') {
            steps {
                script {
                    git url: 'https://github.com/wakaleo/game-of-life.git', branch: 'master'
                }
            }
        }

        stage('Build Game of Life') {
            steps {
                script {
                    sh 'mvn package'
                }
            }
        }

        stage('Archive Artifacts Game of Life') {
            steps {
                archiveArtifacts 'gameoflife-web/target/*.war'
            }
        }

        stage('Publish Test Results Game of Life') {
            steps {
                junit '**/surefire-reports/*.xml'
            }
        }

        stage('Use JDK 21 for Spring Petclinic') {
            steps {
                script {
                    // Specify the correct path to JDK 21
                    def jdkPath = '/usr/lib/jvm/java-21-openjdk-amd64'
                    sh "export JAVA_HOME=${jdkPath} && export PATH=${jdkPath}/bin:${PATH}"
                }
            }
        }

        stage('Checkout and Build Spring Petclinic') {
            steps {
                script {
                    // Specify the directory for spring-petclinic
                    def springPetclinicDir = 'spring-petclinic'

                    // Checkout Spring Petclinic in the specified directory
                    dir(springPetclinicDir) {
                        stage('Checkout Spring Petclinicit'){
                        git url: 'https://github.com/spring-projects/spring-petclinic.git', branch: 'main'
                        }
                        stage('Build Spring Petclinic'){
                        // Build Spring Petclinic
                        def jdkPath = '/usr/lib/jvm/java-21-openjdk-amd64'
                        sh "export JAVA_HOME=${jdkPath} && export PATH=${jdkPath}/bin:${PATH} && mvn package"
                        }
                        stage('Archive Artifacts Spring Petclinic'){
                        // Archive artifacts from the 'spring-petclinic' directory
                        archiveArtifacts "target/*.jar"}
                        stage('Publish Test Results Spring Petclinic'){
                        // Publish JUnit test results from the 'spring-petclinic' directory
                        junit "**/surefire-reports/*.xml"
                        }
                    }
                    
                    
                }
            }
        }
    }
}
