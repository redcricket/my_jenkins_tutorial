pipeline {
    agent {
        docker {
            image 'ansible/ansible:default'
            args '-i --entrypoint='
        }
    }
    stages {
        stage('Pre-Flight (automate MOP review') {
            steps {
                script {
                    def whole_file_data = readFile(file: 'the-mop')

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
                            currentBuild.result = 'FAILURE'
                            return
                        }
                    }
                }
            }
        }
        stage('Confirm pre-flight successful ') {
            steps {
                script {
                    // Define Variable
                    def USER_INPUT = input(
                            message: "To approve and proceed click Proceed",
                            description: 'To approve and proceed click Proceed']])

                    println("user input is =>${USER_INPUT}")
                    if( "${USER_INPUT}" == "OK"){
                        println("Hurray ${User} (certified dick head) approves!")
                    } else {
                        println("${User} shits on your pre-flight. And says fuck your mother!")
                        currentBuild.result = 'FAILURE'
                        return
                    }
                }
            }
        }
    }
}