---
layout: post
title: "Released EasyRSA Gem v0.8.0"
category: "Ruby"
tags: ruby gem easyrsa openvpn openssl certificates
---

### Introduction

Not too long ago I wrote an [article](https://www.mikemackintosh.com/generating-openvpn-client-certs-with-ruby/) about generating OpenVPN certificates with ruby. This method worked great, but didn't leave much room for scalability and mass distribution. To combat this, I created the `EasyRSA` gem, which can be found on [rubygems](https://rubygems.org/gems/easyrsa).

## Defining Your Issuer Details

There is a small configuration block, in which all of the following attributes are required. They are all used to namespace the certificate.

    EasyRSA.configure do |issuer|
      issuer.email = 'support@company.com'
      issuer.server = 'vpnserver.company.com'
      issuer.country = 'US'
      issuer.city = 'New York'
      issuer.company = 'My Company'
      issuer.orgunit = 'IT'
    end

## Generating Your Certificate



