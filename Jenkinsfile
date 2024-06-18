pipeline{
    agent any

    stages{
        stage ('zip the file'){
            steps{
                sh 'rm -rf *.zip || echo ""'
                sh 'zip ansible-${BUILD_ID}.zip * --exclude Jenkinsfile'
            }
        }
        stage('upload artifacts to jfrog'){
            steps{
                sh 'curl -uadmin:AP8gcgmmset5jeYChTJYDN6XmDd -T \
                ansible-${BUILD_ID}.zip \
                "http://ec2-18-204-7-164.compute-1.amazonaws.com:8081/artifactory/ansible-repo/ansible-${BUILD_ID}.zip"'
            }
        }
        stage('Publish to ansible server'){
            steps{
                sshPublisher(publishers: [sshPublisherDesc(configName: 'AnsibleServer', transfers: [sshTransfer(cleanRemote: \
                false, excludes: '', execCommand: 'ls', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: \
                false, patternSeparator: '[, ]+', remoteDirectory: '.', remoteDirectorySDF: false, removePrefix: '', \
                sourceFiles: 'ansible-${BUILD_ID}.zip')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
    }   

}