pipeline {
    agent {
        docker {
            image 'quay.io/ansible/ansible-runner:stable-2.12-latest'
            args '-u root'

        }
    }
        environment {
       ANSIBLE_HOST_KEY_CHECKING ='false'
     }
    stages {
        stage('ansible') {
            steps {
                sh 'ansible --version'
                sshagent(credentials:['centos-private-key']){
                
                sh 'ansible server1 -i hosts -m command -a "cat /etc/os-release" -u ec2-user'
                //sh 'ansible  server1:server3 -i hosts -m ping -u ec2-user'
                
                //sudo yum install tree -y -> es lo mismo que hice abajo
                sh 'ansible server1 -i hosts -m ansible.builtin.yum -a "name=tree state=latest" -u ec2-user --become'
                sh 'ansible server1 -i hosts -m yum -a "name=git state=latest" -u ec2-user --become' // git es del core de ansible, no hace falta el builtin
                sh 'ansible-playbook -i hosts playbooks/sver1_config.yml'
                }
            }
        }
    }
}