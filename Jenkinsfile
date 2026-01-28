pipeline {

    agent { label 'master' }

    tools { jdk 'jdk17' }

    parameters {
        string(name: 'VERSION', defaultValue: '2.8.0-lt-fix', description: 'Docker image version to build and push (empty to skip)')
    }

    stages {
        stage('Building debezium extend jars') {
            steps {
                sh './mvnw clean install -DskipTests'
            }
        }

        stage('Building image') {
            steps {
                dir('./') {
                    script {
                        if (params.VERSION.isEmpty()) {
                            echo "Skipping the Docker image stage"
                            return
                        }
                        def app = docker.build ("docker-new.finnplay.net/clickhouse-sink-connector:${params.VERSION}","--build-arg appVersion=${params.VERSION} --pull --no-cache .")
                        app.push()
                    }
                }
            }
        }
    }
}
