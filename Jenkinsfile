pipeline {
    agent any

    environment {
        MAVEN_HOME   = tool name: 'Maven-3.9.11', type: 'maven'
        JAVA_HOME    = tool name: 'JDK-21', type: 'jdk'
        TOMCAT_URL   = 'http://localhost:8080/manager/text'
        TOMCAT_CRED  = credentials('tomcat-admin-cred')
        WAR_NAME     = 'roshambo.war'
        APP_CONTEXT  = '/roshambo'  // URL path of your app
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Undeploy Old WAR') {
            steps {
                echo "Removing old deployment..."
                sh """
                    curl -u ${TOMCAT_CRED_USR}:${TOMCAT_CRED_PSW} \
                    "${TOMCAT_URL}/undeploy?path=${APP_CONTEXT}" || true
                """
            }
        }

        stage('Build WAR') {
            steps {
                echo "Building WAR..."
                withEnv(["PATH+MAVEN=${MAVEN_HOME}/bin", "JAVA_HOME=${JAVA_HOME}"]) {
                    sh 'mvn clean package'
                }
            }
            post {
                success {
                    echo "Build successful — starting deployment..."
                    script {
                        sh """
                            curl -u ${TOMCAT_CRED_USR}:${TOMCAT_CRED_PSW} \
                            --upload-file target/${WAR_NAME} \
                            "${TOMCAT_URL}/deploy?path=${APP_CONTEXT}&update=true"
                        """
                    }
                }
                failure {
                    echo "Skipping deployment — build failed!"
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful!"
        }
        failure {
            echo "❌ Deployment failed!"
        }
    }
}
