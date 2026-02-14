https://www.youtube.com/playlist?list=PLdpzxOOAlwvIZ7u-gtpX_bozrspUbTQ1S --> Shell Scripting Playlist ( Abhishek )

https://github.com/iam-veeramalla/ultimate-linux-guide/  -->  Ultimate Linux Guide ( Abhishek )

https://github.com/iam-veeramalla/a-to-z-of-networking/  -->  A to Z Networking ( Abhishek )


1. what shell scripting u did in your project ?

we had several ssl certs imported into AWS ACM . these certs were imported from Cloufare registrar . however these certs had a 365 days expiration policy . hecne we need to renew these certs periodically . we had schell scripts created for this which would generate the csr for the domain and send it to cloudfare registrar through api post . then cloufare would sigh the csr and give us the website chain & body file . the script would then upload these to aws acm to renew the certificates . if the scripts would fail for any reason we would get emails for it 

What is the purpose of #!/bin/bash or #!/bin/sh?

What is the difference between ksh, bash and dash?

How to execute a Shell Script?

How to grant permissions in Linux?

chmod command and how to use?

How to check CPU and RAM of a Linux Machine?

What is "Top" command and why is it used?
