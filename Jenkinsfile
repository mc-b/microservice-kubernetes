pipeline {
    agent none
    stages {
        stage('Build') {
		    agent {
		        docker {
		            image 'maven:3-alpine'
		            args '-v /root/.m2:/root/.m2'
			    }
		    }         
            steps {
                sh 'cd microservice-kubernetes-demo/ && mvn -B -DskipTests clean package'
                archiveArtifacts 'microservice-kubernetes-demo/**/*.jar'
                stash includes: 'microservice-kubernetes-demo/**/*.jar', name: 'jar'
            }
        }
        stage('Build Images') { 
        	agent any
            steps {
            		unstash 'jar'
					sh 'docker build -t misegr/microservice-kubernetes-demo-apache:v0.0.1 microservice-kubernetes-demo/apache'
					sh 'docker build -t misegr/microservice-kubernetes-demo-catalog:v0.0.1 microservice-kubernetes-demo/microservice-kubernetes-demo-catalog'
					sh 'docker build -t misegr/microservice-kubernetes-demo-customer:v0.0.1 microservice-kubernetes-demo/microservice-kubernetes-demo-customer'
					sh 'docker build -t misegr/microservice-kubernetes-demo-customer:v0.0.1 microservice-kubernetes-demo/microservice-kubernetes-demo-customer'
					sh 'docker build -t misegr/microservice-kubernetes-demo-order:v0.0.1 microservice-kubernetes-demo/microservice-kubernetes-demo-order'
					sh 'docker build -t misegr/microservice-kubernetes-demo-hystrix-dashboard:v0.0.1 microservice-kubernetes-demo/microservice-kubernetes-demo-hystrix-dashboard'
            }
        }
        stage('Test') {
		    agent {
		        docker {
		            image 'maven:3-alpine'
		            args '-v /root/.m2:/root/.m2'
			    }
		    }           
            steps {
                sh 'cd microservice-kubernetes-demo/ && mvn test'
            }
            post {
                always {
                  junit 'microservice-kubernetes-demo/**/surefire-reports/*.xml'
                }
            }
        }        
    }
}
