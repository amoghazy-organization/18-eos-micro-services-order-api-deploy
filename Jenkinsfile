    def env = "prod"
  pipeline {
    agent {
        kubernetes {
            label 'agent'
            defaultContainer 'build'
        }
    } 
      stages  {

                 stage ('Checkout SCM'){
             steps {
            git credentialsId: 'git', url: 'https://github.com/amoghazy-organization/18-eos-micro-services-order-api-deploy.git', branch:  "main"
          }}


           stage ('Helm Chart') {
             steps {
            container('build') {
                withCredentials([usernamePassword(credentialsId: 'jfrog-cred', usernameVariable: 'username', passwordVariable: 'password')]) {
                      sh '/usr/local/bin/helm repo add eos-helm-local  https://triallekevd.jfrog.io/artifactory/ecom-helm-local --username $username --password $password'
                      sh "/usr/local/bin/helm repo update"
                      sh "/usr/local/bin/helm upgrade  --install --force order-api  --namespace ${env} --create-namespace -f values.yaml eos-helm-local/order-api"
                      sh "/usr/local/bin/helm list -a --namespace ${env}"
                      sh "rm -rf values.yaml"
              }
          }}
          }
      }
  }

