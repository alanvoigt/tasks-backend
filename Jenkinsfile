pipeline {
	agent any	
	stages {
		stage ('Build Backend') {
			steps {
				bat 'mvn clean package -DskipTests=true'
			}
		}
		stage ('Unit Tests'){
			steps {
				bat 'mvn test'
			}
		}
		stage ('Deploy backend') {
			steps {
				deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
			}
		}
		stage ('apitest'){
			steps {
				dir('api-test'){
					git 'https://github.com/alanvoigt/api-test.git'
					bat 'mvn test'
				}
			}
		}
		stage ('deploy frontend'){
			steps {
				dir('frontend'){
					git credentialsId: 'f66d30ab-2ef6-4b63-bf74-90977aee3343', url: 'https://github.com/alanvoigt/tasks-frontend.git'
					bat 'mvn clean package'
					deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001')], contextPath: 'tasks-frontend', war: 'target/tasks-frontend.war'
				}
			}
		}
		stage ('functionaltest'){
			steps {
				dir('functional-test'){
					git credentialsId: 'f66d30ab-2ef6-4b63-bf74-90977aee3343', url: 'https://github.com/alanvoigt/tasks-functional-test.git'
					bat 'mvn test'
				}
			}
		}
	}
}
