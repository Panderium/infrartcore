# Infr'art Core

## Install and setup Fedora CoreOs VM on GCP
```bash
# Generate ignition file from butane file
envsubst < config.bu | butane --pretty --strict > config.ign

# Create external statique IP
gcloud compute addresses create infrart --region northamerica-northeast1

# Launch a new VM on GCP
gcloud compute instances create infrart-core \
    --image-family=fedora-coreos-stable \
    --image-project=fedora-coreos-cloud \
    --metadata-from-file=user-data=config.ign \
    --machine-type=e2-medium \
    --zone=northamerica-northeast1-a \
    --address=infrart \
    --tags=infrart-core,k3s-master \
    --boot-disk-size=20GB \
    --boot-disk-type=pd-balanced


# Open firewall port for http, https and ssh
gcloud compute firewall-rules create allow-http-https-ssh \
  --allow tcp:80,tcp:443,tcp:22 \
  --target-tags=infrart-core,k3s-master \
  --source-ranges=0.0.0.0/0 \
  --description="Allow HTTP, HTTPS and SSH traffic"
```