---
layout: post
slug: Decrypting TLS with Wireshark
---

Decrypting a TLS session using Wireshark could not be more easier. Recently I've been investing a lot of my time into understanding TLS, both the history and also the implementation. In this blog post I want to show you how you can use Wireshark to decrypt a TLS session.

I've found that there are two ways which is either importing the server side private key into Wireshark or using the TLS session key to decrypt that particular traffic. In some smaller environments accessing the private key for the web server is trivial but when your troubleshooting something more robust or an external web server often you won't have the luxury of having access to the private key because after all, private keys need to stay private for a reason!

With that being said, If you have access to a private key here is where you can stash it in Wireshark:

![wireshark](https://raw.githubusercontent.com/jonathanlynn/jonathanlynn.github.io/master/images/wireshark-menu-1.png){:.ioda}

Simply define the IP address, Port, Protocol (ie http) and the key file (and optionally password) and your off! It will stay valid and decrypt any TLS session your client establishes to that IP address until that key expires and or is rotated.

If you don't have access to the private keys which is the case for many if not all external web servers then we need to focus on the TLS session keys found here:

![wireshark](https://raw.githubusercontent.com/jonathanlynn/jonathanlynn.github.io/master/images/wireshark-menu-2.png){:.ioda}

A quick way of doing this on a Windows 10 client is to:

Create an environment variable in System Preference with the below information:

![wireshark](https://raw.githubusercontent.com/jonathanlynn/jonathanlynn.github.io/master/images/wireshark-menu-3.png){:.ioda}

Configure wireshark to look at the premaster key here:

![wireshark](https://raw.githubusercontent.com/jonathanlynn/jonathanlynn.github.io/master/images/wireshark-menu-4.png){:.ioda}

Kick off a capture, close and re-open your browser and start browsing to somewhere. In  this example I visited Microsoft.com and you can see Wireshark is decrypting the TLS session and showing me the HTTP traffic:

![wireshark](https://raw.githubusercontent.com/jonathanlynn/jonathanlynn.github.io/master/images/wireshark-menu-5.png){:.ioda}

---

Feel free to reach out at my email to leave feedback and talk about the article.

---