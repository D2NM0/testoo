# DCGM-exporter

This subrepo aims at deploying NVidia's DCGM exporter using ArgoCD

It uses the [official, base chart](https://github.com/NVIDIA/dcgm-exporter/tree/main/deployment) with overloaded values 

## Deployment

There are two apps and four applicationsets: one app for each ArgoCD, and one ApplicationSet for each env.
The apps have to be deployed manually once, from there, they will handle the life of the ApplicationSets.

The chart is deployed using ApplicationSets. Once the applicationset is deployed on ArgoCD, adding `obs-dcgm-exporter/enabled: true` annotation to an ArgoCD cluster will automatically create an application containing the DCGM exporter and its requirements

## Environment

For run simplicity, the ApplicationSets have been duplicated so that there's one for each environment. This way, any upgrade can be done individually on the concerned env instead of having to roll it out everywhere at once.

Doing so requires changing the target revision of the official chart in the corresponding ApplicationSet

## Values

You'll find the value at the root of the subrepo, with rough comments explaining the keys' purposes.
