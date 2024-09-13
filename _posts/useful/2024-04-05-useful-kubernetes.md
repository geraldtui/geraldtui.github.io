---
layout: post
title: Useful Kubernetes
date: 2024-04-05 07:59
category: [Useful]
tags: [kubernetes, k8s, useful]
---

Useful commands, shortcuts, configs and setups for Kubernetes

# Shell Setup
```bash
alias k="kubectl"
alias v="vim"
alias kgp="kubectl get pods"
alias kgn="kubectl get nodes"

# usage: k run nginx --image=nginx $dr > nginx.yaml
export dr="--dry-run=client -oyaml"

# usage: k delete pod nginx $now
export now="--force --grace-period=0"

# For setting default namespace
# usage: ns kube-system
function ns () {
  kubectl config set-context --current --namespace=$1
}
```
{: file="~/.bashrc" }

# VIM Setup
```bash
# nu = set line numbers
# ts = tab stop and sw = shiftwidth - use 2 spaces instead of 1 for tab
# ai = autoindent on newline
set nu ai ts=2 sw=2 expandtab
```
{: file="~/.vimrc" }

# Shortcuts
```bash
# Create a yaml file for creating an nginx pod which uses the nginx image
k run nignix --image nginx $dr > pod_template.yaml

# Creating pods
k run messaging-pod --image=redis:alpine --labels=tier=msg

# Create namespace
k create ns my-new-namespace

# Get existing pods current yaml manifest
k get pod <pod-name> --export -o yaml

# Create a pod using a yaml manifest
k apply -f orange.yaml

# Expose a given port on a pod
k expose pod messaging --port=6379 --name messaging-service
```
