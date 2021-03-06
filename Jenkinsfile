pipeline{
    agent{ node {label 'master' }}
    options{
        buildDiscarder(logRotator(numToKeepStr: '2'))
        disableConcurrentBuilds()
        disableResume()
        timestamps()
    }
    environment{
	DEPLOY_TO = 'prod'
    }
    parameters{
        string(name:'NAME',defaultValue:'',description:'person name')
        booleanParam(name:'MALE',defaultValue:false,description:'person gender')
        choice(name:'AGE',choices:['15-20','21-25','26-30','31-35'],description:'person age range')
    }
    stages{
        stage('stage-1'){
            steps{
                retry(3){
                    sh 'echo Hello'
                }
                sh 'echo $WORKSPACE'
            }
        }
        stage('stage-2'){
            when{
                environment name:'DEPLOY_TO', value:'prod'
                allOf{
                    expression{ params.NAME ==~ /(Biswa|BJD)/ }
                    expression{ params.AGE == '21-25' }
                }
            }
            steps{
                echo "Environment variable: ${env.DEPLOY_TO}"
                echo "Name is ${params.NAME}"
                echo "Male: ${params.MALE}"
                echo "Age group: ${params.AGE}"
            }
        }
        stage('stage-3'){
            input{
                id "stage-2-input"
                message "Shell we proceed?"
                ok "Yes, we should"
                submitter "alice,bob"
                parameters{
                        string(name:'FNAME',defaultValue:'',description:'submitter name')
                        choice(name:'TYPE',choices:['admin','root','user'],description:'type of submitter')
                }
            }
            steps{
                sh 'echo Input is submitted by ${FNAME} and he is ${TYPE}'
            }
        }
    }
}
