pipeline{
    agent{
        label "nodejs"
    }
    environment {
        OPENSHIFT_SERVER = "https://api.eu46r.prod.ole.redhat.com:6443/"
        OPENSHIT_NAMESPACE = "jenkins"
        OPENSHIFT_TOKEN = credentials('Openshift-jenkins-account')
    }
    stages{
        stage("Install dependencies"){
            steps{
                sh "npm ci"
            }
        }

        stage("Check Style"){
            steps{
                sh "npm run lint"
            }
        }

        stage("Test"){
            steps{
                sh "npm test"
            }
        }

        // Add the Release stage here
        stage('Release') {
            steps {
                script {
                    sh '''
                        oc login --token=${OPENSHIFT_TOKEN} --server=${OPENSHIFT_SERVER} "
                        oc project jenkins
                        oc start-build greeting-console  --follow --wait
                    '''
                }
             }
        }
    }
}
