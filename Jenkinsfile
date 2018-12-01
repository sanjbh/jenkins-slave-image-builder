pipeline {
  
    environment {
        registry = "asia.gcr.io/sage-ace-224107/jenkins-slave-builder"
        commitId = env.GIT_COMMIT.substring(0,6)
    }
  
    agent {
        label 'image-builder'
    }
  
    stages {
        stage('Build') {
            steps {              
                withCredentials([file(credentialsId: 'gcr-secrets-file', variable: 'GC_KEY')])  {
                    container('jenkins-slave-image-builder') {
                        sh '''
                            docker build -t "${registry}:${commitId}" .
                            cat ${GC_KEY} | docker login -u _json_key --password-stdin https://asia.gcr.io
                            #docker login -u _json_key -p $(cat ${GC_KEY}) https://asia.gcr.io
                            docker push ${registry}:${commitId}
                        '''
                    }
                }
            }
        }
    }
}
