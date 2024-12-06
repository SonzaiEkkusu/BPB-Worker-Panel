# <h1 align="center">Installation via Cloudflare Pages with Upload</h1>

## Introduction
You probably know that there are two methods for using Workers and Pages to create a proxy on Cloudflare. The interesting point is that the Worker method, which is more common, has a limitation where it does not allow more than 100,000 requests per day. However, this limit is sufficient for 2-3 users. To bypass this limitation in the Worker method, we used to attach a domain to the Worker, which allowed for unlimited requests (this was apparently a bug in Cloudflare). However, Pages does not have this limitation (recently, some reports have suggested that this method may also have limitations, so you should test it yourself). Since we are using a feature called Pages Functions in this method, you will still receive an email notifying you that the 100k usage limit has been reached. Even if you use a custom domain, you will still receive this email. **But, experience has shown that your service will not be interrupted.**

## Step 1 - Cloudflare Pages
If you don’t have a Cloudflare account, create one [here](https://dash.cloudflare.com/sign-up) (you only need an email to register; due to Cloudflare’s security requirements, it’s recommended to use a reliable email like Gmail).

Download the Worker ZIP file [here](https://github.com/bia-pain-bache/BPB-Worker-Panel/releases/latest/download/worker.zip).

Now, in your Cloudflare account, go to the left sidebar and enter the `Workers and Pages` section, click `Create Application`, and select `Pages`:

<p align="center">
  <img src="assets/images/Pages_application.jpg">
</p>

Here, click `Upload assets` and proceed to the next step.
Give your project a `Project Name` which will be the domain for your panel. Choose a name that doesn’t include "bpb" as it may cause Cloudflare to identify your account. Click `Create Project`. In this step, you need to upload the ZIP file you downloaded, so click `Select from computer`, choose `Upload zip`, and upload the file. Finally, click `Deploy site`, then `Continue to project`.

Your project has now been created, but it is still not usable. From this `Deployment` page, click `visit` under `Production`, and you will see an error saying you need to set the UUID and Trojan Password first. There is a link there, open it in your browser and leave it open for the next step.

<p align="center">
  <img src="assets/images/Generate_secrets.jpg">
</p>

## Step 3 - Create Cloudflare KV and Configure UUID and Trojan Password
From the left menu, go to the `KV` section:

<p align="center">
  <img src="assets/images/Nav_dash_kv.jpg">
</p>

Click `Create a namespace`, give it a name, and click Add.

Go back to the `Workers and Pages` section and enter the Pages project you created. Go to the `Settings` section as shown in the image below:

<p align="center">
  <img src="assets/images/Settings_functions.jpg">
</p>

Here, similar to the Worker method, find the `Bindings` section, click `Add`, select `KV Namespace`, and for `Variable name`, enter `bpb` (exactly as written). For `KV namespace`, choose the KV you created in the previous step, and click `save`.

<p align="center">
  <img src="assets/images/Pages_bind_kv.jpg">
</p>

Now, we've completed the KV setup.

In the same `Settings` section, you will see `Variables and Secrets`. Click `Add variable`, enter `UUID` in the first field in uppercase, then copy and paste the `UUID` value from the link you opened earlier and click `Save`. Do this again for `TROJAN_PASS` (in uppercase), and paste the Trojan password from the same link and click `Save`.

Now, click `Create deployment` at the top of the page and upload the ZIP file again, as you did before.

Now, you can go back to the `Deployments` page, and under `Production`, click `visit`. Add `panel/` at the end of the URL and enter the panel.
Further setup guides and notes can be found in the [main guide](configuration_fa.md).
The installation is complete, and the following sections may not be necessary for most users!

<br><br>
<h1 align="center">Advanced Settings (Optional)</h1>

## 1- Fixing Proxy IP:

We have an issue where, by default, this code uses a large number of Proxy IPs, selecting a new IP randomly each time it connects to websites behind Cloudflare (which includes a large portion of the web). As a result, your IP changes intermittently. This change in IP might be problematic for some users (especially traders). To fix the Proxy IP from version 2.3.5 onwards, you can configure it directly from the panel by applying it and updating the sub. However, I recommend using the method described below because:

> [!CAUTION]
> If you apply a Proxy IP through the panel and that IP becomes inactive, you will need to replace it and update the sub. This means that if you’ve provided a config and change the Proxy IP, it won’t work anymore since the user does not have a sub to update the config. Therefore, it is recommended to use this method only for personal use. However, the benefit of the second method is that it’s done through the Cloudflare dashboard, and no config updates are required.

<br><br>

To change the Proxy IP, when you enter the project, go to `Settings` and open `Environment variables`:

<p align="center">
  <img src="assets/images/Pages_env_vars.jpg">
</p>

Here, you need to specify the values. Each time you click `Add`, enter a value and click `Save`:

<p align="center">
  <img src="assets/images/Pages_add_variables.jpg">
</p>

Now, click `Add variable`, enter `PROXYIP` in uppercase, and you can get the IPs from the link below. Open it, and it will show you a list of IPs, where you can also check their countries and choose one or more:

>[Proxy IP](https://www.nslookup.io/domains/bpb.yousef.isegaro.com/dns-records/)

<p align="center">
  <img src="assets/images/Proxy_ips.jpg">
</p>

> [!TIP]
> If you want to use multiple Proxy IPs, you can enter them separated by commas, like `151.213.181.145`, `5.163.51.41`, `bpb.yousef.isegaro.com`.

Now, click `Create deployment` at the top of the page and upload the ZIP file again as you did before, and the changes will be applied.

<br><br>

## 2- Connecting a Domain to Pages:

To do this, go to your Cloudflare dashboard, select your panel under `Workers and Pages`. Then, go to the `Custom domains` section and click `set up a custom domain`. It will ask you to enter a domain (note that you should have already purchased and activated a domain for this, which is not covered in this guide). Now, suppose you have a domain named `bpb.com`, you can enter either the domain itself or a subdomain, like `xyz.bpb.com`. Then, click `Continue` and on the next page, click `Activate domain`. Cloudflare will automatically connect the Pages service to your domain (it might take up to 48 hours for this to happen). After this time, you can access your panel via `https://xyz.bpb.com/panel` and manage new subs.

<br><br>

<h1 align="center">Panel Update</h1>

To update the panel, just download the latest ZIP file [here](https://github.com/bia-pain-bache/BPB-Worker-Panel/releases/latest/download/worker.zip). Go to your Cloudflare account, enter the `Workers and Pages` section, and go to the Pages project you created. Click `Create deployment` at the top of the page and upload the new ZIP file as you did initially, and that's it.
