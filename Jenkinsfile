node{ 
    stage("Git clone"){   
        git credentials: 'GIT_HUB_CREDENTIALS', url: 'https://github.com/anhdotdo/kubernetes-full-stack-example_ando.git'   
    }   
     
    stage("Maven build"){   
        dir("spring-boot-student-app-api"){   
            sh 'echo "maven build image"'   
            sh 'mvn -v'  
            sh 'whoami'  
            //sh 'sudo mvn package'  
        }   
    }   
     
    stage("Docker build"){   
        dir("react-student-management-web-app"){   
            sh 'echo "docker build image"'   
            //sh 'sudo docker version'   
            //sh 'sudo docker image list'   
            sh 'sudo docker build -t student-app-client .'   
            sh 'sudo docker tag student-app-client anhdo98/student-app-client:v7005'   
        }   
    }   
     
    withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD')]){   
        sh 'sudo docker login -u anhdo98 -p $PASSWORD'   
    }   

    stage("Push image to docker hub"){   
        sh 'sudo docker push anhdo98/student-app-client:v7005'   
    }   
    
    stage("Deploying front-end, back-end, mongo-db"){   
        sh 'helm version' 
        sh 'sudo helm upgrade --install mychart-rel mychart'   
        sh 'sudo kubectl get svc -A'   
    }   
     
    stage("Istio helm charts"){ 
        sh 'sudo helm upgrade --install istio-base istio/base -n istio-system' 
        sh 'sudo helm upgrade --install istiod istio/istiod -n istio-system --wait' 
        sh 'sudo helm upgrade --install istio-ingress istio/gateway -n istio-ingress' 
        sh 'echo "Expose service to public"' 
        dir("my-istio"){ 
            sh 'sudo kubectl apply -f ./student-gateway.yaml' 
        } 
    } 
     
    stage("Deploy Prometheus and Grafana"){ 
        sh 'sudo helm repo add stable https://charts.helm.sh/stable' 
        sh 'sudo helm repo update' 
         
        //don't need to upgrade 
        //sh 'sudo helm upgrade --install prometheus-operator-1659002484 stable/prometheus-operator' 
         
        sh 'echo "Modify the type ClusterIP to LoadBalancer "' 
        //sh 'sudo kubectl edit svc prometheus-operator-165900-prometheus' 
        //sh 'sudo kubectl edit svc prometheus-operator-1659002484-grafana' 
        sh 'echo "Test the URL of these services"' 
        sh 'sudo kubectl get svc -n default' 
        sh 'sudo minikube service prometheus-operator-165900-prometheus -n default' 
        sh 'sudo minikube service prometheus-operator-1659002484-grafana' 
    } 
} 
