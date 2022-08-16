def whole_file_data

pipeline {
    agent any
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
                        } else if (line.startsWith("CHKLOG|"))  {
                            println("No pre-flight needed for CHKLOG.")
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
                            runAnsibleAction(line)
                        } else if (line.startsWith("ANSIBLEPLAYBOOK|"))  {
                            runAnsiblePlaybookAction(line)
                        } else if (line.startsWith("CHKLOG|"))  {
                            checkLog()
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
    // def (ACTION, PATTERN, INVENTORY, LIMIT, MODULE, DASH_A, EXTRA_PARAMS) = line.tokenize('|')
    def (ACTION, PATTERN, INVENTORY, EXTRA_PARAMS) = line.tokenize('|')
    println("${ACTION}, ${PATTERN}, ${INVENTORY}, [${EXTRA_PARAMS}]")
    build job: 'run_ansible', propagate: true, wait: true, parameters: [
        [$class: 'StringParameterValue', name: 'PATTERN', value: PATTERN],
        [$class: 'StringParameterValue', name: 'INVENTORY', value: INVENTORY],
        // [$class: 'StringParameterValue', name: 'LIMIT', value: LIMIT],
        // [$class: 'StringParameterValue', name: 'MODULE', value: MODULE],
        // [$class: 'StringParameterValue', name: 'DASH_A', value: DASH_A],
        [$class: 'StringParameterValue', name: 'EXTRA_PARAMS', value: EXTRA_PARAMS + ' --list-hosts']]
}

def checkAnsiblePlaybookAction(String line) {
    println("checkAnsiblePlaybookAction called with  line = ${line}")
    def (ACTION, INVENTORY, LIMIT, PLAYBOOK, EXTRA_PARAMS) = line.tokenize('|')
    build job: 'run_ansibleplaybook', propagate: true, wait: true, parameters: [
        [$class: 'StringParameterValue', name: 'INVENTORY', value: INVENTORY],
        [$class: 'StringParameterValue', name: 'LIMIT', value: LIMIT],
        [$class: 'StringParameterValue', name: 'PLAYBOOK', value: PLAYBOOK],
        [$class: 'StringParameterValue', name: 'EXTRA_PARAMS', value: EXTRA_PARAMS + ' --list-hosts']]
}

def runAnsibleAction(String line) {
    println("runAnsibleAction called with  line = ${line}")
    def (ACTION, PATTERN, INVENTORY, LIMIT, MODULE, DASH_A, EXTRA_PARAMS) = line.tokenize('|')
    println("${ACTION}, ${PATTERN}, ${INVENTORY}, ${LIMIT}, ${MODULE}, [${DASH_A}], ${EXTRA_PARAMS}")
    build job: 'run_ansible', propagate: true, wait: true, parameters: [
        [$class: 'StringParameterValue', name: 'PATTERN', value: PATTERN],
        [$class: 'StringParameterValue', name: 'INVENTORY', value: INVENTORY],
        [$class: 'StringParameterValue', name: 'LIMIT', value: LIMIT],
        [$class: 'StringParameterValue', name: 'MODULE', value: MODULE],
        [$class: 'StringParameterValue', name: 'DASH_A', value: DASH_A],
        [$class: 'StringParameterValue', name: 'EXTRA_PARAMS', value: EXTRA_PARAMS]]
}

def runAnsiblePlaybookAction(String line) {
    println("runAnsiblePlaybookAction called with  line = ${line}")
    def (ACTION, INVENTORY, LIMIT, PLAYBOOK, EXTRA_PARAMS) = line.tokenize('|')
    build job: 'run_ansibleplaybook', propagate: true, wait: true, parameters: [
        [$class: 'StringParameterValue', name: 'INVENTORY', value: INVENTORY],
        [$class: 'StringParameterValue', name: 'LIMIT', value: LIMIT],
        [$class: 'StringParameterValue', name: 'PLAYBOOK', value: PLAYBOOK],
        [$class: 'StringParameterValue', name: 'EXTRA_PARAMS', value: EXTRA_PARAMS]]
}

def checkLog() {

    def logFile = input(
        id: 'userInput', message: 'Enter log filename :?',
        parameters: [
            string(defaultValue: 'None',
                description: 'Log file to check.',
                name: 'logFile'), ]
    )
    println("Check log file ${logFile}")
    build job: 'run_chklog', propagate: true, wait: true, parameters: [
        [$class: 'StringParameterValue', name: 'LOGFILE', value: logFile]]
}