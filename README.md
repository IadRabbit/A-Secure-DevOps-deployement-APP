# A Secure DevOps deployment APP

Write-UP [here](https://omniscient-leech-632.notion.site/ASDDA-an-EKS-CTF-556d8d26760247a0bbc6fda278d92a94?pvs=4)


> [!CAUTION]
> Advises Setup this infrastructure has cost

- EKS alone cost 0.10$ per hour
- EC2 t3.small per month cost around 0.0208$ per hour
- The Public IP for the ALB cost 0.005$ per hour

So be careful to tear down all when you finish


## Setup the infra
Clone the repo & go inside `/infra/pre-infra/` folder. This is gonna create a bucket where we are gonna store our `tfstate`

> [!IMPORTANT]
> Bucket name are unique so if the apply fails probably you need to add random chars to the bucket name in the `s3.tf` files before the apply


```bash
cd /infra/pre-infra/
```

Apply

```bash
terraform apply
```

Go back. Now we are gonna create the entire infrastructure.
> [!IMPORTANT]
> The infra is gonna use the default vpc that has already internet access so be careful to don't modify it. The apply is gonna take a while, around 15 minutes.

```bash
cd .. && terraform apply
```

Once the infra is up let update our `.kube/config`

```bash
aws eks update-kubeconfig --name a-super-ultra-secure-company-cluster --alias a-super-ultra-secure-company-cluster
```

Go in `test_4_local` dir

```bash
kubectl apply -f asdda-deploy.yml
```

The deploy should have also created an ingress that in auto create a ALB

```bash
aws elbv2 describe-load-balancers | jq -r .LoadBalancers.[0].DNSName
```

Have fun & learn ;)