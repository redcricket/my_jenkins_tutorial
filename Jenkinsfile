def whole_file_data

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
                        } else if (line.startsWith("ANSIBLE|"))  {
                            checkAnsibleAction(line)
                        } else if (line.startsWith("ANSIBLEPLAYBOOK|"))  {
                            checkAnsiblePlaybookAction(line)
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
                    whole_file_data.split('\n').each { String line ->
                        if(line.startsWith('#')) {
                            println("Ignoreing comment:${line}")
                        } else if (line.startsWith("ANSIBLE|"))  {
                            println("Executing ANSIBLE >:${line}")
                            // build job: '<Project name>', propagate: true, wait: true
                            // build job: '<Project name>', parameters: [[$class: 'StringParameterValue', name: 'param1', value: 'test_param']]
                            // ANSIBLE|all|INVENTORY|LIMIT|MODULE|COMMAND|EXTRA_ARGS
                            /*

                            string( defaultValue: 'all', name: 'PATTERN', trim: true ),
                            string( defaultValue: 'hosts', name: 'INVENTORY', trim: true ),
                            string( defaultValue: '*', name: 'LIMIT', trim: true ),
                            string( defaultValue: 'shell', name: 'MODULE', trim: true ),
                            string( defaultValue: 'pwd' ,name: 'DASH_A', trim: true ),
                            string( defaultValue: '--list-hosts', name: 'EXTRA_PARAMS', trim: true )
                            */
                            def (ACTION, PATTERN, INVENTORY, LIMIT, MODULE, DASH_A, EXTRA_PARAMS) = line.tokenize('|')
                            println ("${ACTION}, ${PATTERN}, ${INVENTORY}, ${LIMIT}, ${MODULE}, ${DASH_A}, ${EXTRA_PARAMS}")
                            build job: 'run_ansible', propagate: true, wait: true, parameters: [
                                [$class: 'StringParameterValue', name: 'PATTERN', value: PATTERN],
                                [$class: 'StringParameterValue', name: 'INVENTORY', value: INVENTORY],
                                [$class: 'StringParameterValue', name: 'LIMIT', value: LIMIT],
                                [$class: 'StringParameterValue', name: 'MODULE', value: MODULE],
                                [$class: 'StringParameterValue', name: 'DASH_A', value: DASH_A],
                                [$class: 'StringParameterValue', name: 'EXTRA_PARAMS', value: EXTRA_PARAMS]
                            ]
                        } else if (line.startsWith("ANSIBLEPLAYBOOK|"))  {
                            // ANSIBLEPLAYBOOK|INVENTORY|LIMIT|PLAYBOOK|OTHER_ARGS
                            def (ACTION, INVENTORY, LIMIT, PLAYBOOK, EXTRA_PARAMS) = line.tokenize('|')
                            println("${ACTION}, ${INVENTORY}, ${LIMIT}, ${PLAYBOOK}, ${EXTRA_PARAMS}")
                            build job: 'run_ansible', propagate: true, wait: true, parameters: [
                                [$class: 'StringParameterValue', name: 'INVENTORY', value: INVENTORY],
                                [$class: 'StringParameterValue', name: 'LIMIT', value: LIMIT],
                                [$class: 'StringParameterValue', name: 'PLAYBOOK', value: PLAYBOOK],
                                [$class: 'StringParameterValue', name: 'EXTRA_PARAMS', value: EXTRA_PARAMS]
                            ]
                        } else {
                            println("ERROR Unhandle verb >:${line}")
                            currentBuild.result = 'ABORTED'
                            error('Pre-fligth FAILED! Aborting! Look above for error message.')
                        }
                    }
                }
            }
        }
    }
}

def checkAnsibleAction(String line) {
    println("checkAnsibleAction called with  line = ${line}")
}

def checkAnsiblePlaybookAction(String line) {
    println("checkAnsiblePlaybookAction called with  line = ${line}")
}
