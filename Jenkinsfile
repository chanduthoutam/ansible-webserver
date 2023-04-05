pipeline {
    agent { label "agentfarm" }
    stages {
        stage('Delete the workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Installing Ansible') {
            steps {
                script {
                    // def ansible_exists = fileExists '/usr/bin/ansible'
                    if (fileExists('/usr/bin/ansible')) {
                        echo "Skipping Ansible Install - already exists"
                    } else {
                        sh 'sudo apt-get update -y && sudo apt-get upgrade -y'
                        sh 'sudo apt install -y wget tree unzip ansible python3-pip python3-apt'        
                    }
                }
            }
        }
        stage('Download Ansible Code') {
            steps {
                git credentialsId: 'git-repo-creds', url: 'git@github.com:chanduthoutam/ansible-webserver.git'
            }
        }
        stage('run ansible-lint against playbook') {
            steps {
                // # cytopia/ansible-lint:4 - container name and version 4
                sh 'docker run --rm -v $WORKSPACE/playbooks:/data cytopia/ansible-lint:4 apache-install.yml'
                sh 'docker run --rm -v $WORKSPACE/playbooks:/data cytopia/ansible-lint:4 website-update.yml'
                sh 'docker run --rm -v $WORKSPACE/playbooks:/data cytopia/ansible-lint:4 website-test.yml'
            }
        }
        stage('Send slack notification') {
            steps {
                slackSend color: 'warning', message: "Mr. Dude: Please approve ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.JOB_URL} | Open>)"
            }
        }
        stage('Request Input') {
            steps{
                input 'Please approve or deny this build'
            }
        }
    }
}