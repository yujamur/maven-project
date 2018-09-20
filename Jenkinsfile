pipeline{
agent 'master'
	stages{
		agent{
			node{
			label 'master'
			}
		}
		parameters{
		string{defaultValue}
    }
		stage('Compilar') {
		steps{
		node(label:'master') {
		checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'gitHubYujamur', url: 'https://github.com/yujamur/maven-project.git']]])
		sh 'mvn clean package'
		archiveArtifacts artifacts: '**/*.war', onlyIfSuccessful: true
		}
    }
}
	stage('Checkstyle') {
			agent{
			node{
			label 'master'
			}
		}
	steps{
		node(label:'master') {
		sh returnStdout: true, script: 'mvn checkstyle:checkstyle'
		//checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
		checkstyle canComputeNew: false, canRunOnFailed: true, defaultEncoding: '', failedTotalAll: params.MAX_ERRORS, healthy: '', pattern: '', unHealthy: ''
       } 
    } 
}
	stage('Despliegue') {
	agent{
		node{
		label 'Windows'
		}
	}
	steps{
		node(label:'Windows') {
        copyArtifacts excludes: '$TOMCAT_HOME', filter: '**/*.war', fingerprintArtifacts: true, flatten: true, projectName: 'Pipelineascode', selector: specific('$BUILD_NUMBER')
		//copyArtifacts filter: '**/*.war', fingerprintArtifacts: true, flatten: true, projectName: 'Pipelineascode', selector: workspace(), target: '$TOMCAT_HOME'
		} 
		}
	}
	}
}
