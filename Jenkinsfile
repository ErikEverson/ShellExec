/*
 * Jenkinsfile
 * ShellExec
 *
 * Declarative pipeline script - https://jenkins.io/doc/book/pipeline/
 */

pipeline {
    agent any

    options {
        // https://jenkins.io/doc/book/pipeline/syntax/#options
        buildDiscarder(logRotator(numToKeepStr: '100'))
        disableConcurrentBuilds()
        timeout(time: 1, unit: 'HOURS')
        timestamps()
    }

    triggers {
        // cron('H */4 * * 1-5')
        githubPush()
    }

    stages {
        stage('Assemble') {
            steps {
                sh './gradlew assemble --no-daemon --stacktrace'
            }
        }
        stage('Test') {
            steps {
                sh './gradlew test --no-daemon --stacktrace'
            }
        }
        stage('Lint') {
            steps {
                sh './gradlew lint --no-daemon --stacktrace'
            }
        }
        stage('Danger') {
            steps {
                sh 'bundle install --verbose'
                sh 'bundle exec danger --verbose'
            }
        }
    }
}
