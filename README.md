# getGcpIps
#Shell Scripts for GCP to pull/search External and Internal IPs

#Ephemeral IPs
gcloud --format="value(networkInterfaces[0].accessConfigs[0].natIP)" compute instances list --project


#Static IPs
gcloud compute addresses list --project=bigcommerce-production --filter="xx.xx.xxx.xx"


#List All static IPs in Gcloud Bigcommerce Project
for i in $(gcloud projects list --format='value(project_id)'); do echo project_id:$i; gcloud compute addresses list --format='value(name,address)' --project $i --quiet; done; 2> /dev/null >> gcpStaticIPs.csv

#List All ephemeral IPs in gcloud BigCommerce 
for i in $(gcloud projects list --format='value(project_id)' --quiet); do echo project_id:$i; gcloud -q --format="value(networkInterfaces[0].accessConfigs[0].natIP)" compute instances list --project $i --quiet; done; 2> /dev/null >> gcpEphemeralIPs_new.csv

#Search static IP in Gcloud Project
for i in $(gcloud projects list --format='value(project_id)'); do gcloud compute instances list --filter=“xx.xx.xxx.xx” --project=$i  --quiet; done;

#Search ephemeral IP in Gcloud Project
for i in $(gcloud projects list --format='value(project_id)'); do gcloud --format="value(networkInterfaces[0].accessConfigs[0].natIP)" compute instances list  --filter=“xx.xx.xxx.xx”  --project=$i     --quiet; done;
