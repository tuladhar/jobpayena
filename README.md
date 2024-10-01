# [Jobpayena](jobpayena.fyi) - Tracking Tech Jobs in Nepal
[Jobpayena.fyi](jobpayena.fyi) tracks tech jobs in Nepal along with 100% anonymous salary data collection for a transparent tech industry, where professionals are fairly compensated for their skills.

> [!NOTE]
> The jobs are tracks directly from company websites, and for now not relying on third-party job portals.

## Demo
https://github.com/user-attachments/assets/10e3f274-cc87-4e16-a483-c6d487c32fab

## NocoDB

The project uses custom build (with branding removed) of [NocoDB](https://github.com/nocodb/nocodb?tab=readme-ov-file) to power the job listing along with form submission. SQlite3 is used for backend database.

## CloudFlare
For frontend, the project uses combination of NocoDB iframe of Bases to show the dashboard which is then served via CloudFlare pages.

## Hosting

The application is hosted on Hetzner (previously at DigitalOcean, check my migration post on [Twitter (X)](https://x.com/ptuladhar3/status/1841082935635771558?s=61) and [LinkedIn](https://www.linkedin.com/posts/ptuladhar3_kamal-activity-7246852881809633281-B0T8?utm_source=share&utm_medium=member_desktop)

### Pricing Comparison

Here’s a simple comparison table of the lowest-end VPS offerings:

| Provider      | Plan        | Price (Hourly) | Price (Monthly) | Specs                           |
|---------------|-------------|----------------|-----------------|---------------------------------|
| DigitalOcean  | Basic       | $0.006         | $4              | 512 MB RAM / 1 vCPU / 10 GB SSD / 500 GB transfer |
| Hetzner       | CX22       | $0.007         | $5              | 4 GB RAM / 2 vCPU / 40 GB SSD / 20 TB transfer   |

> I must acknowledge that while DigitalOcean's interface is sleek and minimalist, it simply cannot compete with Hetzner when it comes to pricing and server specifications.

## Deployment

Deployment is powered by [Kamal](https://kamal-deploy.org/), it just works. During the migration from DO to Hetzner, it took <2 minutes to fully deploy the app from scratch on empty VM, with following command:

```sh
kamal setup --skip-push --verbose 0.0.2
```

For subsequent deploy of new version, follow the steps:

### 1. Build the custom NocoDB container image and push to Docker Hub.
```
./build-local-docker-image.sh

NEW_VERSION=0.0.3
docker tag nocodb-local ptuladhar/jobpayena:$NEW_VERSION
docker push ptuladhar/jobpayena:$NEW_VERSION
```

>[!IMPORTANT]
> Kamal expects the Docker image to be labelled with name of the service as defined in `config/deploy.yaml`.

### 2. Run kamal deploy
```sh
kamal deploy --skip-push --verbose $NEW_VERSION
```
