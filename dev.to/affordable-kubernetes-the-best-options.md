---
title: 'Affordable Kubernetes: The best options'
published: false
description: Highlighting some of the cheapest reliable options for deploying and running Kubernetes.
tags: kubernetes, devops, terraform, linux
cover_image: https://raw.githubusercontent.com/hayone1/codejourney-blog/main/public/images/covers/affordable-kubernetes-the-best-options.png
canonical_url: https://codejourney.io/blog/affordable-kubernetes-the-best-options
---

Kubernetes brought democracy to the cloud somewhat. Anyone in the community can suggest a feature by writing a [Kubernetes Enhancement Proposal (KEP)](https://www.kubernetes.dev/resources/keps/). You can deploy a cluster on several clouds and choose how agnostic or locked-in you become.
That's great, but it's still the cloud, and the cloud is still expensive. This may be discouraging to someone who's trying to learn Kubernetes for real, or a startup that just wants an affordable test environment. The good news is that there are several affordable options to stand up a functional and secure k8s cluster.

## What are your options?

{% details TL;DR %}

|                  | [OrangePi 5 Pro](https://www.aliexpress.com/item/1005006801741948.html) | [Hetzner](https://www.hetzner.com/cloud/regular-performance/) | [Contabo](https://contabo.com/en/vps-performance/) |
|------------------|----------------|---------|---------|
| Setup Difficulty | ⬤ ⬤ ⬤ ⬤ | ⬤ ⬤ | ⬤ ⬤ ⬤ |
| Cost (test env)  | $350 for 16GB RAM, 8 CPU<br>(one time purchase) | $80 for 16GB RAM, 8 CPU<br>(postpaid monthly) | $45 for 24GB RAM, 8 CPU<br>(prepaid monthly) |
| Best for         | labs, test env | labs, test-prod env | labs, test-prod env |

{% enddetails %}

![Line chart of total cost over 12 months for a 16GB, 8 CPU test environment. The OrangePi stays flat at $350 after the one-time purchase. Hetzner grows by $80 a month and passes the OrangePi in month 5. Contabo grows by $45 a month and passes the OrangePi in month 8.](https://raw.githubusercontent.com/hayone1/codejourney-blog/main/public/images/diagrams/affordable-kubernetes-the-best-options-cost.png)

Firstly, it's important to remove any stereotypes that tell you that production environments can only be deployed on a major hyperscaler or some super expensive self-maintained data center. There are several organizations that were able to reduce their entire cloud bill from tens of thousands to a few thousand dollars because they changed their mindset. That said, let's get into the options.

## OrangePi

This is one of the more powerful cousins of the Raspberry Pi, with a lot more options. It has the most expensive initial cost among the options. For about $350 you can get a board with 8 CPUs and 16GB RAM like the [Orange Pi 5 Pro](https://www.aliexpress.com/item/1005006801741948.html), [Orange Pi 5 Max](https://www.aliexpress.com/item/1005007356107781.html) and [Orange Pi 6 Plus](https://www.aliexpress.com/item/1005010157378785.html). If you have a higher budget, you could also get 32GB variants like this [Orange Pi 6 Plus](https://www.aliexpress.com/item/1005012530475330.html).
You'd also need to get a router, network cable, power cable and charging brick that meets the power demand of the unit you have.

> ⚡ Meeting the power requirement is very essential, or your unit just won't run.

### My OrangePi Setup

I use my OrangePi cluster to test fairly heavy workloads like the [LGTM+ stack](https://grafana.com/about/grafana-stack/) running in microservices mode. This setup should give an idea of a practical configuration, although the final price you get might vary.

#### Physical Components

| Item | QTY | Total Price |
|------|-----|-------------|
| [Orange Pi 5 Pro (8 CPU, 16GB RAM)](https://www.aliexpress.com/item/1005006800863351.html) | x4 | $1,264.33 |
| [M2.5 Series Hex Brass Column](https://www.aliexpress.com/item/1005006160291558.html) | x1 | $11.73 |
| [Wire Zip Ties](https://www.aliexpress.com/item/1005007294796796.html) | x1 | $1.32 |
| [SanDisk Extreme Pro Micro SD](https://www.aliexpress.com/item/1005013515583073.html) | x4 | $63.84 |
| [KingSpec SSD (128GB)](https://www.aliexpress.com/item/1005004405900182.html) | x4 | $129.81 |
| [Orange Pi 5 Pro Acrylic Case](https://www.aliexpress.com/item/1005006989183547.html) | x4 | $27.17 |
| [Orange Pi 5 Pro Copper Case](https://www.aliexpress.com/item/1005010379307213.html) | x4 | $89.05 |
| [Cooling Fan 30 x 30 x 7mm (5V)](https://www.aliexpress.com/item/1005008525558891.html) | x4 | $11.96 |
| [8-Port Ethernet Network Switch](https://www.aliexpress.com/item/1005008830706618.html) | x1 | $20.96 |
| Broadband router (already had one) | x1 | - |
| **Total Price** | | **$1,620.17** |

I had also bought a [10 Ports USB 3.0 HUB Charging](https://www.aliexpress.com/item/1005004910531555.html) at some point. I paused using it for several months, but when I finally decided to use it again, it didn't work.

> 💡 If an item is no longer available from the linked seller, it can probably be bought from another seller.

#### Pi K8s Setup

I installed [Ubuntu 26.04](https://github.com/mack42/OrangePi5Pro) on each server, initially on the SD cards, then on the SSD drives. The easier option is to install the OS only on the SD card, as it allows you to use the SSD fully for persistent volume storage.

I installed k3s using a modified version of the [k3s-ansible](https://github.com/k3s-io/k3s-ansible) project. I also used Cilium for the CNI, load balancer and Gateway API, but you'd be fine using the defaults.

I installed external-dns + cert-manager and integrated them with Azure DNS (I bought the domain from Hostinger and [delegated it to Azure](https://learn.microsoft.com/en-us/azure/dns/dns-delegate-domain-azure-dns)). At this point, I could only access the cluster when on the local network, but I didn't have to deal with certificate issues or mess with host entries.

For access over the internet, I bought a cheap VPS on Contabo, linked it to the Pi servers using [SSH reverse tunneling](https://pinggy.io/blog/ssh_reverse_tunnelling/) and exposed it via TLSRoute (k3s was also installed on the Contabo server).

That method wasn't really neat, so what I'd rather advise is to use [fast reverse proxy](https://github.com/fatedier/frp), which will link the 2 clusters (the Pi cluster and the one on the server).

> Tailscale can also do the job.

![How a reader reaches the OrangePi cluster: the browser connects to a public Contabo VPS running k3s, and an frp tunnel (or SSH reverse tunnel, or Tailscale) carries the traffic through the home router and switch to the four Orange Pi nodes. On the local network, devices reach the nodes directly.](https://raw.githubusercontent.com/hayone1/codejourney-blog/main/public/images/diagrams/affordable-kubernetes-the-best-options-pi-access.png)

## Hetzner

Hetzner is the most mature and production-ready option, thanks to community contributions. One regular performance server with 8 vCPU and 16GB RAM - [CPX42](https://www.hetzner.com/cloud/regular-performance/) will cost ~$80.

### My Hetzner Setup

I mostly use Hetzner for short end-to-end tests when I need a throwaway server. Though you can buy domains from Hetzner's [domain registration robot](https://www.hetzner.com/registrationrobot/), I still bought my domain from Hostinger and delegated it to Hetzner's nameservers.

I use the [hcloud-k8s](https://github.com/hcloud-k8s/terraform-hcloud-kubernetes) Terraform module to set up the cluster, but you can also use [hetzner-k3s](https://github.com/vitobotta/hetzner-k3s). A good note for secure access here is that it comes with Tailscale support.

## Contabo

Contabo is a really great option once you understand its quirks. For example, payment is prepaid, so you can't just spin up and tear down servers as you wish. You can cancel a server's subscription, but it will stay active till the end of its billing period since you already paid in full. Similar to Hetzner, there are also 3 main pricing tiers. The [Core VPS](https://contabo.com/en/vps/) is subject to heavy sharing on CPU cores, disk I/O and network links. [Performance VPS](https://contabo.com/en/vps-performance/) works similarly but uses newer AMD EPYC processors. [Max Performance VPS (VDS)](https://contabo.com/en/vps-dedicated/) offers full compute isolation and is what you'll want to use for production.

Contabo doesn't have feature parity with Hetzner, but it has all the features you need to create a cluster.

### My Contabo Setup

I personally use Contabo in production. Like Hetzner, you can [buy domains from Contabo](https://contabo.com/en/domains/), but I prefer to buy mine from Hostinger and delegate it to Contabo's nameservers. Why? Because that's the cheaper option.

The Terraform module I use is [contabo-kubernetes](https://registry.terraform.io/modules/hayone1/kubernetes/contabo/latest). It's the only module available. It also comes with support for Tailscale/Headscale integration. One major difference in the architecture compared to the Hetzner setup is that your load balancer will essentially be one or more of the nodes rather than a dedicated cloud load balancer service.
You can also use [k3s-ansible](https://github.com/k3s-io/k3s-ansible) if Ansible is your preferred route.

![Load balancing on Hetzner and on Contabo. On Hetzner, traffic from the internet reaches a managed Hetzner Load Balancer, which spreads it over the cluster nodes. On Contabo, traffic reaches the public IP of one or more cluster nodes, and that node acts as the load balancer for the rest of the cluster.](https://raw.githubusercontent.com/hayone1/codejourney-blog/main/public/images/diagrams/affordable-kubernetes-the-best-options-load-balancer.png)

## Wrapping up

When you cut out most of the marketing noise, you'd find that there are several cost-effective ways to do many things, including deploying Kubernetes. What about stateful workloads like databases, you say? We'll explore that in another article, which I'll link here once it's ready.

*Originally published at [codejourney.io](https://codejourney.io/blog/affordable-kubernetes-the-best-options).*
