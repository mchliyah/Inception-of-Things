# Inception-of-Things


Kubernetes project focused on building and deploying local `k3s` environments with `Vagrant` and Kubernetes manifests.

## Overview

This repository is organized into three main parts plus an optional bonus setup:

- `P1`: a two-node `k3s` cluster using `Vagrant`
- `P2`: application deployment with Kubernetes manifests and ingress routing
- `P3`: automation scripts for setting up tooling and a local cluster workflow
- `bonus`: additional configuration for GitLab and Argo CD integration

## Repository Structure

```text
README.md
P1/
	Vagrantfile
P2/
	Vagrantfile
	app1.yaml
	app2.yaml
	app3.yaml
	ingress.yaml
P3/
	scripts/
		install_first.sh
		run.sh
bonus/
	confs/
		gitlab_dep.yaml
	scripts/
		app1.yaml
		run.sh
```

## Prerequisites

Before running any part of the project, make sure you have:

- `Vagrant`
- `VirtualBox`
- a Linux host with `sudo` access
- `kubectl` available for Kubernetes interactions when needed

## Part 1: k3s Cluster with Vagrant

`P1` provisions two Debian Bookworm virtual machines:

- one master node at `192.168.56.110`
- one worker node at `192.168.56.111`

The master installs `k3s`, exports the cluster token, and the worker joins the cluster using that token.

### Run

```bash
cd P1
vagrant up
```

## Part 2: Deployments and Ingress

`P2` contains Kubernetes manifests for three applications and one ingress resource:

- `app1.yaml`: Nginx deployment and service
- `app2.yaml`: Apache HTTP Server deployment with 3 replicas
- `app3.yaml`: BusyBox-based HTTP responder
- `ingress.yaml`: host-based routing for `app1.com`, `app2.com`, and `app3.com`

The `P2/Vagrantfile` sets up a `k3s` master node, waits for the cluster to be ready, and applies the manifests automatically.

### Run

```bash
cd P2
vagrant up
```

After deployment, the applications are intended to be reachable through the ingress configuration.

## Part 3: Automation Scripts

`P3/scripts` contains helper scripts for environment setup:

- `install_first.sh`: installs Docker-related dependencies and prepares the host
- `run.sh`: installs `k3d`, creates a cluster, installs Argo CD, and sets up port forwarding

### Run

```bash
cd P3/scripts
chmod +x install_first.sh run.sh
./install_first.sh
./run.sh
```

## Bonus

The `bonus` folder contains GitLab-related configuration and a setup script that installs GitLab, reconfigures it, and updates the Argo CD application manifest.

### Files

- `bonus/confs/gitlab_dep.yaml`: Kubernetes deployment and service for `wil-playground`
- `bonus/scripts/app1.yaml`: application manifest used by the bonus workflow
- `bonus/scripts/run.sh`: GitLab installation and Argo CD update script

## Notes

- Most scripts and manifests assume a local lab environment.
- Some values such as IP addresses and hostnames are hardcoded for the project setup.
- You may need to adjust host DNS entries or `/etc/hosts` depending on how you access the ingress hostnames.