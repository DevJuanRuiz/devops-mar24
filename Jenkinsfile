pipeline {
    agent any
 
    stages {
        stage('SCM') {
            steps {
                git branch: 'master' , url: 'https://github.com/DevJuanRuiz/devops-mar24'
            }
        }
        
        stage('Builder Image') {
            steps {
              bat(script: 'docker login --username %UserDocker% --password %PasswordDocker%' ,returnStdout:true); 
              bat(script: 'docker build -t juanruiz210494/devops-mar24 .' ,returnStdout:true);
              bat(script: 'docker push juanruiz210494/devops-mar24' ,returnStdout:true);
            }
        }
        
        stage('CD AKS') {
            steps {
              bat(script: 'az login --service-principal --username bc71eac8-3e78-4645-80d5-b3c12818ad86 --password yPM8Q~YLgZlB0XgN03JGFTCcD6.X8z5JAPx4Edzz --tenant b1118921-8996-4153-a684-736b654d61b1' ,returnStdout:true); 
              bat(script: 'az account set --subscription da931e00-bd8e-412b-a0f6-bddecb4b31fd' ,returnStdout:true);
              bat(script: 'az aks get-credentials --resource-group Devops --name devopsmar24 --overwrite-existing' ,returnStdout:true);
              bat(script: 'kubectl config get-contexts --kubeconfig=%KUBE_PATH_CONFIG%' ,returnStdout:true);
              bat(script: 'kubectl config use-context devopsmar24 --kubeconfig=%KUBE_PATH_CONFIG%' ,returnStdout:true);
              bat(script: 'kubectl apply -f k8s.yml --kubeconfig=%KUBE_PATH_CONFIG%' ,returnStdout:true);
              bat(script: 'kubectl rollout restart deployment app-deployment --kubeconfig=%KUBE_PATH_CONFIG%' ,returnStdout:true);
            }
        }
    }
}
