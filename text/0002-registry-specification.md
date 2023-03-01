# Specification

- Start Date: 2023-03-01
- RFC PR: fluxed-lang/rfcs#2
- Flux Issue:

## Contents

1. [Summary](#summary)
2. [Motivation](#motivation)
3. [Web API](#web-api)
	- [API Endpoints](#api-endpoints)
	- [Pushing Packages](#pushing-packages)
4. [Storage Format](#storage-format)

## Summary

This RFC introduces the specification for the Flux Registry, including its web API and storage format.

The registry is a service that provides a searchable index of Flux packages. It is designed to be stored in both its entirety and as fragments on a developer's local machine.

## Motivation

For the sake of usability and prevention of re-inventing the wheel, it is desirable to allow Flux to import packages from a central repository. This repository should be searchable, and should allow for the discovery of packages that are not already installed on the local machine.

## Web API

### API Endpoints

All endpoints are prefixed with `/v1/` for forward compatibility.

- `GET /v1/packages/:namespace/:name` - Returns an index of all versions of a package
- `GET /v1/packages/:namespace/:name/:version` - Get a package by namespace, name, and version

- `GET /v1/registry` - Returns a 

### Pushing Packages

Pushing packages requires the authentication and authorization of the user. This is done using a variety of OAuth compatible flows, including:

- Google
- GitHub
- GitLab
- BitBucket

## Storage Format

TODO
