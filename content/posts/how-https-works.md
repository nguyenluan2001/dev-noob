---
title: "How HTTPS works?"
date: 2020-09-15T11:30:03+00:00
weight: 1
# aliases: ["/first"]
tags: ["networking"]
author: "Me"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: true
# description: "Desc Text."
canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
# editPost:
#     URL: "https://github.com/<path_to_repo>/content"
#     Text: "Suggest Changes" # edit text
#     appendFilePath: true # to append file path to Edit link
---
Welcome back! Today I will introduce to you another interesting topic: "How HTTPS works?". Let's get started.

## First of all, what is HTTP?

HTTP stands for Hyper Text Transfer Protocol, It is a protocol used to connect devices on the internet. So, how can devices connect through HTTP?

Take a look at the picture.
![avatar](/photos/how-https-works/1.png)

Basically, the user will create a request to a server with some data; in this case, the data is `{name: John, password: 123}`. When the server receives that request, it will process and respond to the user's information.

Everything works well when a hacker comes.
![avatar](/photos/how-https-works/2.png)

Because HTTP transfers data only in plain text, hackers can catch this data, and it's very dangerous for confidential data such as passwords, credit card numbers, etc. So how do we prevent hackers from stealing data?

## HTTPS coming?
To prevent data from being stolen, it must be encrypted( like a box locked by a key), and anyone who has a key can open a box. HTTPS does the same thing; the `S` letter in HTTPS means `Secure`. HTTPS uses SSL (Secure Sockets Layer) to encrypt data when transferred on the internet, so even if a hacker can catch this data, they can't read it.

Look how HTTPS works.
![avatar](/photos/how-https-works/3.png)

1. The user wants to access the domain example.com via https, so the browser sends a request to the server.
2. The server said, "Ok, I will give you my certificate and public key."
3. The browser receives a certificate and then verifies
4. Once the verification is complete, the browser can now trust the domain.
5. The browser automatically generates a session key, which is then encrypted by a public key that is received from the server.
6. The browser sends that encrypted key to the server.
7. The server uses its private key to decrypt. Now both browser and server have the session key `(Symmetric key)`
8. Whenever a browser sends data to a server, it must be encrypted by a session key, and the server must use the same key to decrypt the data and vice versa.

For those of you who don't know
- `Symmetric key` means using the same key to encrypt and decrypt.
- `Asymmetric key` means using two keys: a public key to encrypt data and a private key to decrypt data.

## Conclusion
After reading this article, you can now understand why HTTPS is used more commonly than HTTP and how it works under the hood. Thank you, and see you in another article.