---
title: Prerequisites
toc: true
---
In order to run Agencms there are a number of steps to prepare your instance.

## Agencms Portal

The Agencms User-Interface is a hosted platform and you will need to ensure you have a Tenant account and your site pre-configured.

We are currently in closed-alpha testing phase so you will need to request access through our [Slack group](https://join.slack.com/t/agencms/shared_invite/enQtMzMyNTExMTg1NjM4LWZiOGFmZTdiYjAzYzdiMjMyNGQ2MjNmZjZjOTI0NGY2MTQzN2U2NTM1MjhhMDk4NjUwZDMyY2I4ZTg2MzAzYjI). You will receive a welcome email with instructions of how to set your password and access the portal.

## Cloudinary Account

The Cloudinary service is used to power the Media Manager within your CMS (a file based driver will also be available in the near future), so a free Cloudinary account is required.

* Get a Cloudinary Account [https://cloudinary.com](https://cloudinary.com)

When you have created your account you will need to configure your `.env` file with the Cloudinary API credentials. See the [configuration documentation](../0201-configuration/) for detais.
