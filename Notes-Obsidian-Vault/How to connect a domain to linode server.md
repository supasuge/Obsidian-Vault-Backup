## Table of Contents

      - [Linode Nameservers](#Linode\Nameservers)
- [Overview of DNS and DNS records](#overview\of\dns\and\dns\records)

`source`:[!Linode](https://www.linode.com/community/questions/19221/how-to-configure-a-domain-name-step-by-step-on-a-linode-server-platform)
Once you have purchased your domain from a domain registrar like GoDaddy or Namecheap, I'd recommend changing the name servers with that provider to use our Linode name servers. This will allow you to use our built in DNS Manager and control your DNS records from one location. This step isn't necessary, but I highly recommend it as it can save time from jumping back and forth between multiple accounts.

Instructions for changing name servers will vary depending which domain registrar you choose but I have provided guides for some of the major ones below, along with a list of the Linode name servers.

[Update Name Servers With Hostinger](https://www.hostinger.com/tutorials/dns/how-to-change-domain-nameservers)  
[Update Name Servers With HostGator](https://www.hostgator.com/help/article/how-to-change-name-servers-with-hover)  
[Update Name Servers With GoDaddy](https://www.godaddy.com/help/set-custom-nameservers-for-domains-registered-with-godaddy-12317)  
[Update Name Servers With Namecheap](https://www.namecheap.com/support/knowledgebase/article.aspx/767/10/how-to-change-dns-for-a-domain)

#### Linode Nameservers

- ns1.linode.com
- ns2.linode.com
- ns3.linode.com
- ns4.linode.com
- ns5.linode.com

Once the name servers has been configured you will need to setup some additional DNS Records. I have included a link to our DNS guides below which will provide information on each type of record and how to configure them in your DNS Manager. For basic websites an `A` and `AAAA` record should suffice.

Quick tip, when first setting up a domain in your DNS Manager take note of the "Insert Default Records" field. This will let you select the backend Linode you want your domain to point to and automatically configure a set of default records. This isn't perfect, but it does the job for basic configurations. Once everything is setup it can take up to 48 hours for everything to propagate globally so don't get discouraged if it does not work right away. I've also included a link to a great third party site you can use to check the propagation status.

[Linode DNS Manager](https://www.linode.com/docs/platform/manager/dns-manager/)  
[Types of DNS Records](https://www.linode.com/docs/networking/dns/dns-records-an-introduction/)  
[Check Propagation Status](https://dnschecker.org/)

Once your site is up and running, you might consider adding an SSL certificate, allowing secure connections between your web server and a connecting browser. Linode does not provide SSL certificates, but we do have an excellent guide on using Certbot which I have included a link to below. Certbot is a tool the will automatically add a signed certificate from [Let's Encrypt](https://letsencrypt.org/) to your web server's files and enable HTTPS.

[Secure HTTP Traffic with Certbot](https://www.linode.com/docs/quick-answers/websites/secure-http-traffic-certbot/)
