# OpenSSL Guide

![image](https://github.com/user-attachments/assets/ec056031-e31e-45e3-b2b9-32b0aca8c18a)

## Internet Information Services (IIS) Certificates

![image](https://github.com/user-attachments/assets/ffd857e3-bb65-41ee-9703-f07b2104d257)

## Follow this tutorial

Downlad latest version of OpenSSL.

[https://www.firedaemon.com/firedaemon-openssl]

That's the latest version, compiled pretty nicely, supports more aes modes.

Use the next command on Terminal, to create a private key for Your IIS Certificate Request.

Terminal icon.

![image](https://github.com/user-attachments/assets/d9134257-d121-4906-b956-b52229d20fd4)

The command being issued, after a while it will ask for a password.

![image](https://github.com/user-attachments/assets/6027701b-0b23-4ccb-b5a5-6852112af8b8)

Specify it two times, same password. It'd better be a good alpha numeric with special symbols password, more than 12 characters, at least. Call me crazy, but better safe than sorry.

![image](https://github.com/user-attachments/assets/a31de240-aa27-4dce-a24f-df316cce4244)


``openssl genrsa -aes256 -f4 -rand .\largefile.iso -out .\ca.key 16384``

Got not a single idea why everybody sets this special key to be broken by a rainbow table attack, and specifies without even knowing -des3, instead of -aes256, des3 has been fully cracked and deprecated for example on [https://github.com/pyca/cryptography]

Here is an image about TripleDES.

![image](https://github.com/user-attachments/assets/990a3ad0-bc73-4171-82a3-d5326f57da40)

About AES 256 strength. No it has bot been cracked, follow the legitimate source always.

https://newatlas.com/quantum-computing/chinese-quantum-computer-hack-rsa-aes-military-grade-encryption/

![image](https://github.com/user-attachments/assets/8947caf2-2293-46c8-85ba-f0c943cccbcb)

``openssl genrsa -aes256 -f4 -rand .\largefile.iso -out .\ca.key 16384``

In case You doubt this, use Camellia of 256 bits, instead.

![image](https://github.com/user-attachments/assets/19179382-42cb-44d8-8b62-88aba98536ee)

![image](https://github.com/user-attachments/assets/a4ce237b-801e-4d6e-a1a4-8112e2608406)

``openssl genrsa -camellia256 -f4 -rand .\file.ext -out .\ca.key 16384``

More about this command, refer to openssl manual.

[https://docs.openssl.org/1.0.2/man1/genrsa/]

I specify -f4 which is to base of 65537, there is base -f1 which is to base 3, and the key is 16384 bits long. To make it more strange and adjust the algorithm to my needs. Also I specify a file for more random generated private key.

![image](https://github.com/user-attachments/assets/6027701b-0b23-4ccb-b5a5-6852112af8b8)

After that click Windows key or icon, and write IIS

Click on the next icon.

![image](https://github.com/user-attachments/assets/c620182b-24e3-4525-a60e-eb8ae429c23d)

On the root of IIS. Double click on Server Certificates.

![image](https://github.com/user-attachments/assets/dd150c38-f894-4d07-9ff5-c276854d02db)

Now click on Create Certificate Request.

![image](https://github.com/user-attachments/assets/272d9b58-de74-422f-801c-b3b661538268)

On the data of the certificate specify exactly what You got on openssl.cnf file. In my case.

![image](https://github.com/user-attachments/assets/164e8ad1-efad-47b5-b8e0-ad7faeb9f8d7)

Then establish it to Microsoft RSA SChannel Cryptographic Provider, set the key length to the bits of the private key which You desire the most. My private key is set to 16384 bits. Which is way too much for a certificate to break, therefore way slower. Set it to 4096 bits, that is more than enough.

![image](https://github.com/user-attachments/assets/50c3a7d5-634b-4f80-a4b8-2e46b2bdd89e)

After that save the request in a txt file. See carefully where You save it.

![image](https://github.com/user-attachments/assets/55e62344-4b04-4054-bf47-5d79545faa81)

I save the file as requestcert.txt

![image](https://github.com/user-attachments/assets/fa9e1f81-3483-47f3-862a-d2d7a83fe987)

These are the partial contents of the txt file.

![image](https://github.com/user-attachments/assets/bd9ab1c1-29f8-4bb6-b8e4-7cbcfb0c5781)

Go back to the Terminal.

After writing two times the password.

The ca.key file will be created.

Now issue this command specifiying on folder the configuration given by yourself in a openssl.cnf file

Get the file on this repository [https://github.com/fochoa8/IIS-Certificate-By-Request/blob/master/openssl.cnf]

![image](https://github.com/user-attachments/assets/26af3660-ba0d-4c84-b552-10cfa1957337)

Download it and place it in a folder where You create Your own certificates.

In my case.

``openssl req -x509 -new -nodes -sha384 -days 365 -key .\ca.key -out .\IISRootCA.crt -config .\openssliis.cnf``

![image](https://github.com/user-attachments/assets/5bce1cdc-1a39-4551-b39f-8b84ce2e555f)

Here is the command that follows.

``openssl pkcs12 -export -out .\RootCA.pfx -inkey .\ca.key -in .\IISRootCA.crt``

![image](https://github.com/user-attachments/assets/70aed272-b683-4b1a-b0c2-be7221521476)

Double click RootCA.pfx and open with Shell Crypto Extensions.

![image](https://github.com/user-attachments/assets/714b863d-1a03-4454-aaaa-ad1f7fa23e25)

In this dialog click Next.

![image](https://github.com/user-attachments/assets/5aa96a00-f9d4-4024-84ad-8dea5628a414)

After that the same, click Next.

![image](https://github.com/user-attachments/assets/e06b63cb-9283-4f81-a57e-24a58a3f92d5)

I use these settings normally.

![image](https://github.com/user-attachments/assets/db910885-501d-49fe-8803-8913e4936b10)

Click Next. Then select second option and click Browse... select, Trusted Root Certificate Authority folder, as certificate import.

![image](https://github.com/user-attachments/assets/55cfda6b-5c06-4960-bcd2-1167223f4223)

![image](https://github.com/user-attachments/assets/db5f147e-bf8f-4aaa-80a5-9d0bcf005a49)

Now click Finish.

![image](https://github.com/user-attachments/assets/91e73bd6-2d4e-4c67-8af7-f3c9ba8e70c0)

It will ask if You want to install the certificate, click Yes.

![image](https://github.com/user-attachments/assets/b70e0c46-4460-4f34-9a61-9ed892c0e2e0)

![image](https://github.com/user-attachments/assets/8d89b4db-0f6e-46f9-9c03-aaa65c048f84)

Now open the file again, do the same steps but in the Browse... step, click on Personal. Then click Next, then Finish.

![image](https://github.com/user-attachments/assets/ab1dc00f-e3d6-4504-a94f-d76c3025b9c7)

![image](https://github.com/user-attachments/assets/a827cff6-7ed3-4f6a-b3a8-9c722d5729a3)

After that repeat the step, except on first dialog, You are going to select Local Machine.

![image](https://github.com/user-attachments/assets/bab100ae-c7b6-418b-b62e-1aaaa045282a)

Then fill this with the same password You entered on last command.

![image](https://github.com/user-attachments/assets/05927b32-96bf-4ca9-85f2-3a8dbabf32cb)

After that place the certificate on Folder named Web Hosting.

![image](https://github.com/user-attachments/assets/fca60896-03de-4f88-8705-859a82e09f8c)

Go back to IIS Server now, restart the server.

![image](https://github.com/user-attachments/assets/50ebf563-416c-464f-916f-db0b7aba5224)

Now click on Server Certificates.

![image](https://github.com/user-attachments/assets/8d89ba47-d6c7-477b-897f-7e8041796720)

After that You will see the certificate right there.

![image](https://github.com/user-attachments/assets/b6ab43c9-a6d2-4c8f-a1ff-b2f7c1d73708)

Now go to sites, and click on Default Web Site. And click on Bindings...

![image](https://github.com/user-attachments/assets/7feda796-d797-45b5-953c-efa3620f196d)

Then click on Add...

Here I only enable TLS in version 1.3 making it harder to crack.

Specify Your own computer name hostname. Mine is razerblade.

Select the SSL Certificate you created.

![image](https://github.com/user-attachments/assets/9ddeec1a-06a9-4d39-8dee-f166bfdaefd0)

Check timestamp and expiration date.

![image](https://github.com/user-attachments/assets/bde2429b-d887-4c66-a323-651cdb381dc8)

If all is working. Press OK. Now Restart the server of Default Web Site.

![image](https://github.com/user-attachments/assets/01138724-5d5a-4c61-abc1-5527398056fc)

Then click on the links on IIS on port 443.

![image](https://github.com/user-attachments/assets/15149ba5-6553-45e4-a59f-8816190f280a)

This is how it looks fully working.

![image](https://github.com/user-attachments/assets/972bc484-528e-4b91-90bd-c22f5dc4b16f)

![image](https://github.com/user-attachments/assets/fa68ac2c-150b-4771-aadf-1fd9b8256bfe)

![image](https://github.com/user-attachments/assets/bce32d59-1efc-4a3a-8f20-065052ea629c)

![image](https://github.com/user-attachments/assets/7f5e5c8d-8e04-4571-8063-d0d115e6ec71)

![image](https://github.com/user-attachments/assets/287fbee4-91d5-4f7b-80f2-035ad56cc31a)

And finally the default site.

![image](https://github.com/user-attachments/assets/1de72fcf-bd9e-4ac1-b32a-c92f235cf4f0)

As well it does not hurt for further securin the device and port 443, which is the only one I expose.

![image](https://github.com/user-attachments/assets/41f44ef8-eb82-4cb4-8556-5fac054d274e)

And now press Windows key, and write Notepad, right click it and press on Run as Administrator...

![image](https://github.com/user-attachments/assets/d740eb86-6a6a-4176-9f7b-bb8ae1bce899)

And now edit hosts file, stored in: C:\Windows\System32\drivers\etc\

![image](https://github.com/user-attachments/assets/d81ba4b4-3f7b-45ea-8b59-0e7d77f9047a)

Write Your own hostnames as You wish.

![image](https://github.com/user-attachments/assets/e5987fc2-f86a-4e43-98b7-0a5ba6b66459)

## Key takeaways

Always use top security for Your production servers.

Let's Encrypt option I never use it, but depends on each person, the end user is the one who will be subject to said decisions.

I made this tutorial so as to show You how to create a total custom Certificate on IIS made with OpenSSL.

## Fernando Ochoa Olivares.
## MIT Fire Hydrant Winner.
