pipeline {
    agent any

    environment {
        SSH_PRIVATE_KEY = credentials('ssh-private-key-plain-text')
    }

    stages {
        stage('Build productive') {
            when {
                branch 'main'
            }
            steps {
                sh 'mkdir -p .ssh'
                sh 'cat ~/.ssh/id_rsa'
                sh 'echo $SSH_PRIVATE_KEY > ~/.ssh/id_rsa'
                sh 'echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC2uFbUuyogXkDcl1OUB39t6iQofUnQHl4r7x3W76nvT8xF0AvMT6Tqb8vtxoD5jebI1WxBMc5oJJHIETepwAW8YLHCEwxEpckaoRtNpUnTYthwpeZEKFMITG5AyA0xTZ6+Qy4BeXGT2EPnZ1r6o8K8KwkTFWBv9Z/dhMgQxcw5PVm2M8f4eal89kPnh0l3w4RXLlO4QkLUNKQCetYfYqym7zkbj+5KQxAm/2o7i8zedVpfAb2sHbSscZawF6harVASo9HyAGPSHWmXxZUj98TzAYu057Ztg+9F1VPGQwlM8agOxojjfNu9m86vk7JQZUAf8kw2hzKC32KaE4YW8fkz jobender@lb01" > ~/.ssh/id_rsa.pub'
                sh 'chmod 600 ~/.ssh/id_rsa'
                sh 'wc -c ~/.ssh/id_rsa'
                sh 'sha1sum ~/.ssh/id_rsa'
                sh 'ssh -o StrictHostKeyChecking=no pipeline@master01.k8s.thejay.azubi.server.lan \'bash -s\' < script.sh'
            }
        }
    }
}