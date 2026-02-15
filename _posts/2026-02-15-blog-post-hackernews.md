# I Built a Retry Script for Oracle's "Out of Capacity" Free Tier Instances

**TL;DR:** Oracle Cloud's free tier ARM instances are amazing but almost impossible to provision due to capacity issues. I built a smart retry script that got mine deployed overnight. [GitHub repo here](https://github.com/ekadetov/oci-terraform-retry-script).

---

## The Problem

Oracle Cloud offers a genuinely generous free tier: 4 ARM CPU cores and 24GB RAM, forever free. It's probably the best free tier out there. There's just one catch — good luck actually getting one.

The ARM instances (VM.Standard.A1.Flex) are so popular that you'll almost certainly hit this error:

```
Error: 500-InternalError
Out of host capacity.
```

People report trying for days, weeks, even months to provision these instances. The manual approach is to spam the "Create Instance" button in the web console every few minutes and hope you get lucky.

## The Terraform Authentication Gotcha

Before I even got to the capacity issue, I hit another roadblock. Running Terraform in OCI Cloud Shell gave me this cryptic error:

```
Error: can not create client, bad configuration: 
did not find a proper configuration for key id
```

After digging through GitHub issues and Oracle docs, I found the fix. Cloud Shell uses a special authentication profile, so your provider block needs to be:

```hcl
provider "oci" {
  config_file_profile = "eu-madrid-1"  # or your region
}
```

Not the `auth = "InstancePrincipal"` that most guides suggest. This took me way too long to figure out.

## The Solution

I wrote a Bash script that wraps `terraform apply` with smart retry logic:

```bash
#!/bin/bash

MAX_RETRIES=60
RETRY_DELAY=60

until [ "$n" -ge "$MAX_RETRIES" ]; do
    OUTPUT=$(terraform apply -auto-approve 2>&1)
    
    # Success? Exit
    if [ $? -eq 0 ]; then
        echo "SUCCESS!"
        exit 0
    fi
    
    # Only retry on capacity errors
    if echo "$OUTPUT" | grep -qi "out of capacity"; then
        echo "No capacity. Retrying in 60s..."
        sleep 60
    else
        # Config error? Stop immediately
        echo "Non-retryable error. Check your config."
        exit 1
    fi
done
```

The key features:
- **Only retries on capacity errors** — stops immediately if there's a config/auth problem
- **Logs everything** — timestamped logs for debugging
- **Runs in background** — use `nohup` to keep it running when you close your terminal
- **Configurable** — adjust retry count and delay to your needs

## Results

I started the script before bed. Woke up to a fully provisioned instance. The logs showed it took 47 attempts over 46 minutes.

```
[2026-02-15 02:47:30] Attempt 47/60
[2026-02-15 02:48:45] ✓ Infrastructure deployed successfully!
```

## Why This Works

Oracle's capacity comes and goes throughout the day as people spin up and tear down instances. The script just sits there waiting for a free slot. It's like having a bot constantly refreshing the page for you.

Some tips:
- **Run it overnight** — less competition from other time zones
- **Try different regions** — some regions have more capacity than others
- **Be patient** — it might take hours, but it works

## The Code

The full script with error handling, logging, and documentation is on GitHub:

**https://github.com/ekadetov/oci-terraform-retry-script**

It includes:
- Full-featured retry script with colored output
- Simple version for minimal setups
- Example Terraform configs
- Complete documentation

## For the HN Crowd

If you're thinking "this is a terrible user experience," you're absolutely right. But the hardware you get for $0/month makes it worth the hassle:

- 4 ARM CPU cores (Ampere Altra)
- 24GB RAM
- 200GB storage
- 10TB/month bandwidth
- Forever free

Compare that to AWS's t2.micro (1 vCPU, 1GB RAM) or GCP's e2-micro (0.25-1 vCPU, 1GB RAM). Oracle's free tier is in a different league.

## Try It Yourself

```bash
git clone https://github.com/ekadetov/oci-terraform-retry-script.git
cd oci-terraform-retry-script
chmod +x terraform_retry.sh
./terraform_retry.sh
```

Good luck, and may the capacity be with you.

---

**Update:** Several people have asked about arm64 Docker compatibility. Yes, these instances run arm64 natively, and Docker works great. I'm running several containers on mine with no issues.

**Update 2:** For those asking about alternative regions — eu-frankfurt-1 and us-ashburn-1 seem to have slightly better availability based on community reports, but YMMV.
