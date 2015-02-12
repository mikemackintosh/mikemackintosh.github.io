---
layout: post
permalink: /?p=1323
title: "Backing Up TimeMachine to a Samba Share"
category: ["mac-osx", "network", "system-administration"]
tags: backups mac osx samba share smb time-machine
---
# The Importance of Backups

# Create Your Share

# Map Your Share

# Adding to TimeMachine

501 /Users/sixeightzero/Desktop/makeimage.sh 130 /Volumes/TimeMachine/mbp 502 sudo /Users/sixeightzero/Desktop/makeimage.sh 130 /Volumes/TimeMachine/mbp 503 defaults write com.apple.systempreferences TMShowUnsupportedNetworkVolumes 1 504 sudo /Users/sixeightzero/Desktop/makeimage.sh 130 /Volumes/TimeMachine-Splug/ 505 cd Desktop/ 506 sudo sh makeimage.sh 130 /Volumes/TimeMachine-Splug/ 507 sudo sh makeimage.sh 130 /Volumes/TimeMachine-Splug/ 508 sudo sh makeimage.sh 130 /Volumes/TimeMachine-Splug/ 509 sudo tmutil setdestination /Volumes/mounted\_sprase\_bundle 510 sudo tmutil setdestination /Volumes/TimeMachine-Splug/ 511 sudo defaults write com.apple.systempreferences TMShowUnsupportedNetworkVolumes 1 512 hdiutil attach -verbose /Volumes/TimeMachine-Splug/ 513 hdiutil attach -verbose /Volumes/TimeMachine-Splug/Splug’s\ MacBook\ Pro.sparsebundle/ 514 sudo tmutil setdestination /Volumes/TimeMachine-Splug/Splug’s\ MacBook\ Pro.sparsebundle/ 515 sudo tmutil setdestination /Volumes/TimeMachine-Splug/ 516 sudo tmutil setdestination /Volumes/Time\ Machine\ Backups/

