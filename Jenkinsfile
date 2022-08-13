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
                        }
                    }
                }
            }
        }
    }
}