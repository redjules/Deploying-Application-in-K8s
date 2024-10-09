# Automate Kubernetes Deployment

![Screenshot 2024-02-29 at 10 47 14](https://github.com/redjules/Automate-Kubernetes-Deployment/assets/106017493/35fb3a8c-f105-492b-b4f9-85da0a200e66)

This project has 2 parts:

- Create EKS cluster with Terraform

- Write Ansible Play to deploy application in a new K8s namespace



# Create EKS cluster with Terraform
  
We do terraform init, terraform plan and terraform apply of our terraform project for EKS (attached):


<img width="513" alt="Screenshot 2024-10-09 at 21 05 56" src="https://github.com/user-attachments/assets/82f19a96-ea2c-46ff-aad0-c6a18b81d019">

to create our EKS cluster in AWS:

![Screenshot 2024-10-09 at 21 08 06](https://github.com/user-attachments/assets/6f992645-79ca-4e50-8555-5ea4abe2efe8)

It’s active and we have 3 nodes:

![Screenshot 2024-10-09 at 21 08 56](https://github.com/user-attachments/assets/307a2c4c-5e16-43b9-9372-00bf8850658b)


With Ansible we want to connect to this cluster and deploy a very simple deployment and service components into that cluster


We create a namespace in EKS Cluster:


First, we need to update our kubeconfig file to be able to connect to our cluster. This kubeconfig file includes all the info needed to connect to our cluster like the server address, the hostname, certificates … and this info is what we’re going to be using to connect to our EKS cluster using Ansible. So first we will generate a kubeconfig file:


<img width="582" alt="Screenshot 2024-10-09 at 21 10 00" src="https://github.com/user-attachments/assets/4658ce65-289c-43c0-91b8-528b7d7aa302">

and here we have the kubeconfig file:


<img width="581" alt="Screenshot 2024-10-09 at 21 10 53" src="https://github.com/user-attachments/assets/7dc8b191-92b7-412f-b22b-1562f22e31cd">


Now we go to our ansible project and create a yaml called deploy-to-k8s.yaml where I will write a plan to deploy an app in a Kubernetes cluster (I will use kubernetes.core.k8s Ansibles module):![image]

<img width="578" alt="Screenshot 2024-10-09 at 21 11 23" src="https://github.com/user-attachments/assets/dd1968ff-d2d1-473d-b948-2bc167a039a1">

We configure Ansible to read the kubeconfig context info from the file to know what the address of the cluster is and use the certificates inside to authenticate with the cluster
We need to fulfill these requirements first:

<img width="558" alt="Screenshot 2024-10-09 at 21 11 49" src="https://github.com/user-attachments/assets/761cfc72-fd59-4537-ad6a-97312c8b26d6">

copy the path of the kubeconfig:

<img width="805" alt="Screenshot 2024-10-09 at 22 16 12" src="https://github.com/user-attachments/assets/b6575388-8587-4613-a038-30cb5b188fd4">


We change the inventory to hosts in our ansible.cfg:

<img width="553" alt="Screenshot 2024-10-09 at 21 13 02" src="https://github.com/user-attachments/assets/bfd1adf9-8cf0-4fa3-a921-3b3961e9b04c">


We save it and execute our ansible playbook:

<img width="378" alt="Screenshot 2024-10-09 at 21 13 32" src="https://github.com/user-attachments/assets/56b96b8d-40ef-45f1-8571-8e7bcf1f5e4f">


We check it:

<img width="548" alt="Screenshot 2024-10-09 at 22 12 42" src="https://github.com/user-attachments/assets/273f7304-cafb-4aa1-971d-38f5e2e6feca">

<img width="265" alt="Screenshot 2024-10-09 at 22 13 59" src="https://github.com/user-attachments/assets/59092c3d-cb1d-49aa-81bc-98cf82a58b35">


Deploy app in the new namespace:

Deployment:

<img width="556" alt="Screenshot 2024-10-09 at 21 14 44" src="https://github.com/user-attachments/assets/570de0d1-569e-4272-935f-432c135afc5b">

Service:

<img width="402" alt="Screenshot 2024-10-09 at 21 15 05" src="https://github.com/user-attachments/assets/40a72830-374b-47b7-97f8-e72ef53b4aac">

We use src: 

<img width="808" alt="Screenshot 2024-10-09 at 22 15 33" src="https://github.com/user-attachments/assets/855b753b-8b87-41b5-b5c8-ac9e9ca93816">


<img width="384" alt="Screenshot 2024-10-09 at 22 46 17" src="https://github.com/user-attachments/assets/a506f1ec-c030-4d14-9e60-8eb97113e3ab">


In our browser we can access our nginx app:


<img width="543" alt="Screenshot 2024-10-09 at 21 16 29" src="https://github.com/user-attachments/assets/40434a75-bb58-4cf2-9af0-91a98ad30a1b">

Set env variable for kubeconfig using K8S-AUTH_KUBECONFIG and delete the kubeconfig entries from our code:
<img width="561" alt="Screenshot 2024-10-09 at 21 17 00" src="https://github.com/user-attachments/assets/459f3827-724e-4c92-8785-8eba12cc765d">


<img width="554" alt="Screenshot 2024-10-09 at 21 17 13" src="https://github.com/user-attachments/assets/b5d9266b-7d59-44f8-b054-deb86a273171">

![Screenshot 2024-10-09 at 22 47 31](https://github.com/user-attachments/assets/a0e43550-97d2-42eb-8c46-de2d6d202459)

And we execute ansible playbook and still connects to the cluster:

<img width="430" alt="Screenshot 2024-10-09 at 21 17 52" src="https://github.com/user-attachments/assets/aa5538bc-0c02-4b68-a043-7a0f19dbcb3c">

