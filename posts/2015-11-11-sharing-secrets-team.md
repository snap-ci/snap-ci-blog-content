---
layout: post
title:  "Sharing Secrets with Teams"
description: "Understanding the problems about not encrypting secrets and discussing the solutions"
date:   2015-11-11
author: Junaid Shah
index-image-url: screenshots/sharing-secrets/snap-ci-i-can-haz-secret.jpg
index-image-alt: 'security for teams'
keywords: vaults, security, encryption, 1password, continuous delivery,
categories: security encryption
---


![sharing secrets with your team securely](/assets/images/screenshots/sharing-secrets/snap-ci-i-can-haz-secret.jpg){: .screenshot .big}

Wouldn't it be awesome if we could share secret information like passwords, keys, and more with collaborators on the team and have them forget it when they leave the project or leave the company? This way, there would be no accidental leakage of secrets outside the team. It would be possible to have people join and leave the team without the nagging worry that we might have accidentally left a door open for them or some evil person that got hold of their machine to gain unauthorized access to the system.

Here’s how the Snap CI team takes care of making sure that is the case.

When we initially started building Snap, individual developers had their own password managers on their workstations, but we had no easy way to share secrets within the team because each developer’s vault was tied to their private encryption key.

So, effectively, we had two problems:

* To create an encrypted store for application secrets
* Have the encrypted store be sharable among collaborators

##Creating an encrypted store for application secrets
Let us first explore why our secrets must be encrypted. After all, we could find a cloud storage service (or an NFS server, if we are old school) and store the secrets there in plain text. The content would be secured behind the credentials we have to use to get access to the service. So why would this be insufficient?

The argument usually goes as follows: let’s say that a new vulnerability discovered tomorrow exposes a backdoor into our super-secure service. It doesn’t matter that the regular means of access was secured. Our data is now toast and available to a malicious interloper. The same thing could happen even if we accidentally allowed access to the content for even a few moments. This is why some security folks often remind us that it is better to build security in layers.

With that in mind, encrypting our secrets now sounds like a good idea. Let's say we use the capabilities of our cloud storage mechanisms to create a secure folder such that any data in and out of it gets automatically encrypted and decrypted on demand. That way, the data stored on the service itself is encrypted at rest. However, if the decryption keys are present on the same system as our content, our malicious interloper could gain access to them both albeit with slightly more effort. Our data, while being slightly more secure, is still potentially accessible depending on how secure the service kept our decryption keys.

Thus the ideal solution for us would be to have a secure server and store all the data on it in encrypted form. In addition, we should also NOT store anything which could be used to decrypt the data on the server.

##Sharing the secured information.
When it comes to encrypting data, the GPG toolchain has the ability to encrypt content with a keychain containing multiple public keys such that any of the private keys corresponding to the public keys in the keychain can be used to decrypt the content. This, however, adds work and responsibility for the team to manage. There are many tools that allow you to use this exact mechanism to store encrypted data on a Version Control System. However in my opinion those tools should only be used when you already have a repository that contains sensitive information which you don't want everyone who can clone your repository to know about.

A much better option would be to never let secrets anywhere near a version control system in the first place.

##A simple solution to sharing encrypted secrets a.k.a. the magic! :)
The Solution that we chose solves the two main problems that we had. It allows us to encrypt the data locally before we store it on any server. We also don’t store any keys or passwords on that server.

Secondly we are also able to share the data between the rest of the team without having the need to maintain our own server to do that or use repositories.

The tool that solves the above problems for us is [1password](https://agilebits.com/onepassword). 1password allows us to create multiple vaults on top of our existing primary vault (the default vault which you have when you install 1password). We can then share these additional vaults with our team. We are using "1password for Teams". This has been recently launched and is still in [beta](https://blog.agilebits.com/2015/11/03/introducing-1password-for-teams/). All the data is stored centrally on AgileBit’s servers and this is how they are able to synchronize the data. You can find more information about the security of 1password for Teams [here](https://teams.1password.com/white-paper/1Password%20for%20Teams%20White%20Paper.pdf).

All the data in the vault is encrypted at the client side before it is stored on the server. The transfer of data between the client and server happens over TLS and there is a client server authentication. The data which was already encrypted locally at rest is also encrypted in transit to the server.

Each user would have their own account key and a master password for the shared vault. The account key is generated and stored on your workstation when you sign up. This account key is used to authenticate to the servers that 1password stores the data on. Both the master password and the account key are used while decrypting the encrypted data. It increases the strength of the password that you would use to authenticate by a huge margin. For example, if someone did manage to hack into the 1Password for Teams server, in order to be able to decrypt the data they would need the account key which resides only on the users workstation before they could launch a password guessing attack against your master password.

In the past we had used the multiple vaults feature of 1password and stored the Vault over a cloud service provider like [Dropbox](https://www.dropbox.com). However we have removed the vault from Dropbox and are now using the 1password for Teams feature. The reason is, that using this new feature we have much more granular control over access to the vault. For instance, we can have read-only users who would only be able to read the secrets on the vault but cannot edit anything on the vault. Also, it eliminates the dependency of using any third party tools like Dropbox or iCloud to sync the vault.

## Some tips and suggestions if you use 1password
1password uses two different data formats. The old format is Agile Keychain and the new format is [OpVault](https://support.1password.com/opvault-design/). There seems to be a problem with the AgileBits format where some of the metadata from the file is accessible. I would recommend changing the data format to OpVault once you have 1password installed. You can follow the instructions mentioned [here](https://support.1password.com/switch-to-opvault/mac.html) to change the data format.

Security is a rapidly evolving field and staying on top of it is a serious matter. We are happy to see the emergence of tools that make it easy for teams to do the right thing and do so easily. If you have any ideas or suggestions then please let us know in the comments section below.

 
Snap CI © 2017, ThoughtWorks
