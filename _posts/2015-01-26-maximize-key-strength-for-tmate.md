---
layout: post
title: Maximize Key Strength for Tmate
tags: tmate pairing ssh keys
---

TMate is a pretty cool tool for sharing shell sessions remotely. I modified the `create_keys` script to read as the following, which maximizes the key strength for the 3 key types:

    #!/bin/bash
    gen_key() {
      bits=4096
      keytype=$1
      ks="${keytype}_"
      key="keys/ssh_host_${ks}key"
      if [ ! -e "${key}" ] ; then
        if [ "${keytype}" = 'dsa' ] ; then
            bits=1024
        elif [ "${keytype}" = 'ecdsa' ] ; then
        bits=521
        fi
        ssh-keygen -t ${keytype} -f "${key}" -b $bits -N ''
        return $?
      fi
    }

    mkdir -p keys
    gen_key dsa && gen_key rsa && gen_key ecdsa || exit 1