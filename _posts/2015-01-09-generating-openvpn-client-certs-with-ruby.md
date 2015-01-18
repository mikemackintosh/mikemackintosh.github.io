---
layout: post
title: Generating OpenVPN Client Certs with Ruby
tags: ruby openssl openvpn gem ssl
---

I was working on a project recently that dealt with generating and revoking SSL certs for OpenVPN clients in a simple and automated fashion, without paying for Access Server. Easy-RSA is easy to use, but totally unsecure to automate (think Shellshock..), and in order to replicate certs for backup, I would need to rsync, GlusterFS or some weird file transfer hacks. In all honesty, it would have been easier to save encrypted certs to a database so the end user can easily pull them down when needed.

To accomplish this without breaking down to a shell or running some kind of `system()` command, I abused the `openssl` gem. The following code snippet, which has been modified from it's original version, can easily be adapted to fit your needs.

        require "openssl"

        # Set Serial Number 
        # -- MUST BE UNIQUE PER USER
        # -- USED TO REVOKE A CERT
        SERIAL_NUMBER = 0x04                    
        # Username
        USER_COMMON_NAME = "Mike"         
        # User Email 
        USER_EMAIL_ADDRESS = "mike@domain.com" 

        # Set these per your org
        COUNTRY = "US"         # Country Code
        STATE = "NY"          # State Name
        LOCALE = "New York"  # City Name
        COMPANY_NAME="<o>"  # Commonly the company name
        OU = "Security"    # Security 
        ISSUER_EMAIL_ADDRESS = "blah@blah.com" #
        ISSUER_SERVER_NAME = "openvpn.blah.com" #

        # Point these to your ca cert and key for openvpn
        CA_CERT_FILE = '/etc/openvpn/keys/ca.crt'
        CA_KEY_FILE = '/etc/openvpn/keys/ca.key'

        # Get CA cert and key
        ca_cert = OpenSSL::X509::Certificate.new File.read CA_CERT_FILE
        ca_key = OpenSSL::PKey::RSA.new File.read CA_KEY_FILE

        # Generate Private Key
        key = OpenSSL::PKey::RSA.new(4096)

        # Let's start to generate a new cert
        cert = OpenSSL::X509::Certificate.new
        
        # Cert subject for End-User
        cert.subject = OpenSSL::X509::Name.parse("/C=#{COUNTRY}/ST=#{STATE}/L=#{LOCALE}/O=#{COMPANY_NAME}/OU=#{OU}/CN=#{USER_COMMON_NAME}/emailAddress=#{USER_EMAIL_ADDRESS}")
        
        # Cert issuer details
        cert.issuer = OpenSSL::X509::Name.parse("/C=#{COUNTRY}/ST=#{STATE}/L=#{LOCALE}/O=#{COMPANY_NAME}/OU=#{OU}/CN=#{ISSUER_SERVER_NAME}/emailAddress=#{ISSUER_EMAIL_ADDRESS}")

        # Cert Details
        cert.not_before = Time.now
        cert.not_after = Time.now + 10 * 365 * 24 * 60 * 60
        cert.public_key = key.public_key
        cert.serial = SERIAL_NUMBER
        cert.version = 2
         
        # Extension Details
        ef = OpenSSL::X509::ExtensionFactory.new
        ef.subject_certificate = cert
        ef.issuer_certificate = ca_cert

        cert.extensions = [
          ef.create_extension("basicConstraints","CA:FALSE"),
          ef.create_extension("nsCertType", "client, objsign"),
          ef.create_extension("nsComment", "Easy-RSA Generated Certificate"),
          ef.create_extension("subjectKeyIdentifier", "hash"),
          ef.create_extension("extendedKeyUsage", "clientAuth"),
          ef.create_extension("keyUsage", "digitalSignature"),
        ]

        cert.add_extension ef.create_extension("authorityKeyIdentifier",
                                               "keyid,issuer:always")
        
        # Sign cert with CA key
        cert.sign ca_key, OpenSSL::Digest::SHA256.new

        # Cert Key
        puts key.to_pem

        # Cert Pem
        puts cert.to_pem

The above will mimic Easy-RSA certificate extensions so OpenVPN can validate a CN.