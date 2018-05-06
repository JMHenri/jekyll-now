---
layout: post

title: You don't need read access to your passwords.

excerpt: A step-by-step breakdown of the permission requirements on a MEAN/MERN server shows that many web-facing servers don't need to fear data exfiltration.

---


I run a small server that needs to be highly secure. I don't have the luxury of a massive amount of free time.

In my case, I'm running a typical, simple MEAN stack, setup on AWS. NGINX was added (and should be added for most people), and provides a simple way to keep track who can connect to what.

However, my architecture suffers deeply from its low work-time resources. Even a single breach to some systems can be catastrophic. I manage blockchain assets; If one private key gets out, that money is gone.

This post lays out a proposed solution for increased user-account authentication, and better separation of privileges. The gist is - read/write authorization is revoked from the server and given to the user.

This solution was never implemented, as my current architecture removes all of the weaknesses that caused me to consider it in the first place.

### Take a look at the permissions of a MEAN web server.

We will consider our database to be on a separate server with a separate control mechanism which is turing complete. In my case, web access in the VPN via Node.js, Express and Mongo.

Consider also that you are running a MEAN server which only needs to handle a user password and email, nothing else.

Reads from the database often need to be passed from the database to the web-server in order to show usernames, emails, and misc. data to users.


However, writes to the database are often authorized from the user via an email. Most simple architectures consider the web server to have this authorization as well, however it is not necessary for such database writes. From this idea, the idea that user 2fa is not just authentication, but authorization. Some separation of privileges and cryptography can be applied in a relatively straightforward manner.

In fact you may want to stop reading, and consider it yourself. What are some of the ways in which you can revoke your access to your critical user data? And does it really get in the way? I said relatively straightforward in the last paragraph because in practice, this design is rather confusing. It is hard to easily understand whether or not your web server does need certain privileges. That is one of the reasons I chose not to implement.

For me the answer was, interestingly, that I can indeed have user-controlled databases.


### Here's how things work in a user-controlled database, and what the advantages are.

##### Password Checking
You never need to read a hash. Only the database-server needs to the read the hash. You can pass the hash to your database server, and get a boolean response. What does this mean in terms of security?
* Data exfiltration - In the case of a catastrophic web-server compromise via an injection or misconfiguration, it will be impossible for your hashes to be exported, as you don't have access to them.

##### Password Resets
Password resets are user-authenticated. If your database-server handles the emails, users never even need to interact with your web-server.

This is a great starting point, but ultimately a bad idea. You'll end up with email reset links that go to funny places, and people won't trust it. On top of that, your database-server shouldn't have any ports accessible to the internet.

This is fine. We've sort of forgotten this, but by sending out reset links, users can authenticate, AND authorize their own password resets. The authorization and authentication cryptography is already handled in the default logic of a password reset.

The database-server can send out a reset link using the web-server as a proxy.
The database-server can receive authorization and authentication by using the web-server as a reverse proxy to retreive the key.

The data must remain encrypted so that the web-server cannot read it. This is a big weak point, because setting up the routing adds confusion and complexity to small architectures.

##### Email Resets

This works the exact same way as a password reset.

##### Displaying user data?

It can be done by setting up an account with read-access on your web-server to your database. This lets you pull out fun information like usernames and emails to keep your site looking up to date and informative. With good permissioning, it will only be able to read what you need it to read.

Setting up a read system like this does destroy one of the two benefits of this separation of duties.
1. ~~My user data is immune to compromise via injections.~~

2. My user data is immune to high-permission server compromise.

This is another reason why I abandoned this. The muddling of security. Each security implementation should be completely unambiguous.
If you choose to not implement read access in any sense, however, then the problem does not exist.


Summary

*You don't need to read your password hashes. You don't need to read your user emails, you don't need to read or write to much of anything, your users do it. It's not easy, and takes an entire day to set up. If you're a small website in desperate need of data-confidentiality. Consider this architecture. This article is merely one way to accomplish this separation.*