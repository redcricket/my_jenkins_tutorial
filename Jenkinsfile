def whole_file_data
def MOP_FILE

pipeline {
    agent any
    //agent {
    //    docker {
    //        image 'ansible/ansible:default'
    //        args '-i --entrypoint='
    //    }
    //}
    stages {
        stage('Setup parameters') {
            steps {
                // PATTERN|INVENTORY|LIMIT|MODULE|DASH_A|EXTRA_PARAMS

                script {
                    properties([
                        parameters([
                            string( defaultValue: 'the-mop', name: 'MOP_FILE', trim: true ),
                        ])
                    ])
                }
            }
        }
        stage('Pre-Flight (automate MOP review') {
            steps {
                script {
                    whole_file_data = readFile(file: MOP_FILE)

                    /********************
                    read line by line:
                    PRIMIERE|INVENTORY|LIMIT|PLAYBOOK|OTHER_ARGS
                    ANSIBLEPLAYBOOK|INVENTORY|LIMIT|PLAYBOOK|OTHER_ARGS
                    ANSIBLE|all|INVENTORY|LIMIT|MODULE|COMMAND|EXTRA_ARGS

                    **************************/
                    whole_file_data.split('\n').each { String line ->
                        if(line.startsWith('#')) {
                            println("Ignoreing comment:${line}")
                        } else if (line.startsWith("ANSIBLE"))  {
                            println("Checking ANSIBLE >:${line}")
                        } else {
                            println("ERROR Unhandle verb >:${line}")
                            currentBuild.result = 'ABORTED'
                            error('Pre-fligth FAILED! Aborting! Look above for error message.')
                        }
                    }
                }
            }
        }
        stage('Approve MOP Step-by-Step') {
            steps {
                script {
                    input "Now what?"
                }
            }
        }
        stage('Execute MOP Step-by-Step') {
            steps {
                script {
                    whole_file_data.split('\n').each { val -> println val }
                }
            }
        }
    }
}