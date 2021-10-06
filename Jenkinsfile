pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
                echo 'haha'
            }
        }
        stage('run tests') {
            parallel {
                stage('test1') {
                    agent {
                        label "gabriel"
                    }
                    steps {
                        echo 'Hello World1'
                        echo "\$(hostname)"
                        echo "\$(id)"
                        sh '''#!/bin/bash
                            echo "$(hostname) $(id)"
                        '''
                    }
                }
                stage('test2') {
                    steps {
                        echo 'Hello World2'
                    }
                }
            }
        }
        stage('Coucou') {
            steps {
                echo 'Coucou World'
            }
        }
    }
}
