pipeline {
	agent {
		docker {
			image 'composer:latest'
		}
	}
	stages {
		stage('Build') {
			steps {
				sh 'composer install'
			}
		}
		stage('Test') {
			steps {
                sh './vendor/bin/phpunit --log-junit logs/unitreport.xml -c tests/phpunit.xml tests'
            }
		}
		stage('Checkout SCM') {
			steps {
				git branch:'master', url:'https://github.com/sxbrinx/jenkinsUnitTesting.git'
			}
		}

		stage('OWASP DependencyCheck') {
			steps {
				dependencyCheck additionalArguments: '--format HTML --format XML', odcInstallation: 'Default'
			}
		}
	}
	post {
		always {
			jnuit testResults: 'logs/unitreport.xml'
		}

		success {
			dependencyCheckPublisher pattern: 'dependency-check-report.xml'
		}
	}
}