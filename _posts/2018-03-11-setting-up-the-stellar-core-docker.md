---
layout: post
title: Setting up the Stellar Core
---

## Stellar Quickstart Docker Image
This project provides a simple way to incorporate stellar-core and horizon into your private infrastructure, provided that you use docker.

This image provide a default, non-validating, ephemeral configuration that should work for most developers. By configuring a container using this image with a host-based volume (described below in the "Usage" section) an operator gains access to full configuration customization and persistence of data.

The image uses the following software:

Postgresql 9.5 is used for storing both stellar-core and horizon data
stellar-core
horizon
Supervisord is used from managing the processes of the services above.
