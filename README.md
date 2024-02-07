Pipeline:



pipeline{
    
    agent any
    
    stages{
        
        stage ('clone github'){
            steps{
                git branch: 'main', url: 'https://github.com/m-alkhalil/docker-ansi-jenkins.git'
            }
        }
        stage ('configure serve with ansible'){
            steps{
ansiblePlaybook credentialsId: 'remoteID', disableHostKeyChecking: true, installation: 'local_ansi', inventory: 'hosts.ini', playbook: 'setupserver.yml', vaultCredentialsId: 'ansiVaultpss', vaultTmpPath: ''            }
        }
        
        stage ("deploy to container") {
            steps{
                ansiblePlaybook credentialsId: 'remoteID', disableHostKeyChecking: true, installation: 'local_ansi', inventory: 'hosts.ini', playbook: 'deploytocont.yml', vaultCredentialsId: 'ansiVaultpss', vaultTmpPath: ''
            }
        }
    }
}
