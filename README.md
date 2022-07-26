## Vanilla wordpress deployment

Hi all!
I hope you are doing well.
To be honest I never had any experience with deploying websites with certmanager. But i reached the following.

## Getting started

1-
  In this first step we`ll set up our enviroment.

  1.1 Download Docker Desktop
  This will be used to run the Minikube.

  1.2 Download Minikube
  This service will be used to deploy the Vanilla Wordpress in containers.

  1.3 Download KubeCtl


  2.
     Setting up the Minikube enviroment:
    2.1
     Commands:

     - minikube start

     This command is used to create our Cluster.

     2.2
       Now we need to create the namespace "wordpress" to deploy the Vanilla Wordpress

     - kubectl create namespace wordpress

3.
   Deploying Wordpress
  To apply the yaml files we have to run the following

   kubectl apply -f ./wordpress/*

  This command will create the wordpress service and the database that is the MySQL

Notes:
  On the kustomization.yaml you will choose your database password.
  
  3.1
    After running the apply command we have to check the URL of our Wordpress Service.
    minikube service wordpress --url -n wordpress
    Now our Wordpress are deployed!!

 ##Working with Cert Manager.
4.
 4.1
  I`ve downloaded the Cert Manager from this link
  https://github.com/jetstack/cert-manager/releases/download/v1.6.1/cert-manager.yaml

  To apply the Cert Manager config

  - kubectl apply -f ./cert-manager.yaml

  Now check if the services of Cert Manager are running

  - kubectl get all -n cert-manager 

4.2
   Create Issuer.yaml this will be responsible to connect with Lets Encrypt to get the certificates.

   Please fill out the Email with your email.

   To expose my Minikube to the internet I used this service called NGROK

   Download this on :
   https://ngrok.com/download

   To expose your minikube to internet run
   - ngrok http IP-OF-YOUR-WEBAPP

4.3
    Create the ingress controller file.
    This yaml will forward your localhost to https on the intert using ngrok

5. Conclusion

   Now you can acess locally your app over the internet and localhost on app-127-0-0-1.nip.io

   When you run the ngrock command you will see this output

#################################################################################
Session Status                online                                                                                        Account                       Gustavo Peters (Plan: Free)                                                                    Version                       3.0.6                                                                                          Region                        South America (sa)                                                                         Latency                      9ms                                                                                           Web Interface                 http://127.0.0.1:4040  
Forwarding                    https://5662-2804-868-d049-236d-d0f9-426b-2eec-a1a4.sa.ngrok.io -> http://127.0.0.1:80
#################################################################################
 So, you can have your site with SSL enabled on
 https://5662-2804-868-d049-236d-d0f9-426b-2eec-a1a4.sa.ngrok.io.


 1.1 Deploying ARGOCD
   
  To deploy argocd on localhost i`ve followed this Documentation

  https://argo-cd.readthedocs.io/en/stable/getting_started/

  Create ArgoCD namespace on Minikube

  - kubectl create namespace argocd

  Please apply the core-install.yaml running this command

  - kubectl apply -n argocd -f core-install.yaml

  To access your Webapp locally run:

   - kubectl port-forward svc/argocd-server -n argocd 8080:443


Questions

1- Describe your approach to setup a CD solution for a Kubernetes environment, 
where there is a need to serve five teams of developers that can’t have direct kubectl access to the cluster and maintain their autonomy to deploy and see the production environment for debugging purposes.
   
   For me the best way is using AWS EKS (Elastic Kubernetes Service) so you can create the develop enviromental, and the production enviroment.
   So you can manage the access of your employees using IAM polices for the developers they will only access the develop enviroment.
   In this way your team will get the full access of the Kubernetes and your production Kubernetes will be safe.


2-How would you manage secrets inside? What tooling would you use?

The tool that I have most knowleged is the AWS Secrets Manager.
In this AWS Service you can manage the secrets of your applications securely and it can be encrypted using AWS KMS





