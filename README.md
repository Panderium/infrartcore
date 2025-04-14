# Infr'art Core

## Install and setup Fedora CoreOs VM on GCP
```bash
# Generate ignition file from butane file
butane config.bu --pretty --strict > config.ign

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


# Open firewall port for http, https and k3s
gcloud compute firewall-rules create allow-http-https \
  --allow tcp:80,tcp:443 \
  --target-tags=http-server,https-server \
  --source-ranges=0.0.0.0/0 \
  --description="Allow HTTP and HTTPS traffic"
```