# Jenkins
##### To bring up jenkins use below command (Note Remember to create dockercred)
> helm repo add jenkins https://charts.jenkins.io

> kubectl create secret docker-registry dockercred --docker-server=https://index.docker.io/v1/ --docker-username=sarthaksatish --docker-password=***** --docker-email=sarthak8055@gmail.com

> helm install --create-namespace jenkins jenkins/