# Deploy a Node / Express app on AWS Lightsail with a Custom Domain + SSL

[Video Tutorial (_10 min_)](https://www.youtube.com/watch?v=rtshCulV2hk)

---

### Steps below if you prefer images

Create a Lightsail instance from the **[AWS dashboard](https://lightsail.aws.amazon.com/ls/webapp/home/instances)**

![Dashboard Image](images/dash1.png)

Choose your instance type

`Linux & Node`

![Instance type](images/instanceType.png)

Wait for the instance to be created

![Wait](images/waiting.png)

In the meantime, go to the `Networking` tab and click on `Create static IP`

![Static](images/static.png)

Attach the static IP you just created to your instance

![attachIP](images/attachIP.png)

Go back [home](https://lightsail.aws.amazon.com/ls/webapp/home/instances) and click on [networking](https://lightsail.aws.amazon.com/ls/webapp/home/networking) so that you arrive at this tab

![networktab](images/networking.png)

Click on `Create DNS zone` and enter your domain here

![domain](images/domain.png)

> I personally use [Google Domains](https://domains.google.com/registrar/) to buy my domains but if you prefer to stay in the ecosystem, Amazon has their own solution called [Route53](https://aws.amazon.com/route53/)

> I will be using `opensourceme.app` for the rest of this tutorial as I have this one for testing

Click `create` and add two `A` records

The first one should be `@` in the `Subdomain` and `Resolves to` should be the static IP address we created a few steps ago

![rs](images/arcrd1.png)

Your second subdomain should be `www` in the first box and once more, select your static IP

![added](images/addedARecords.png)

Add the name servers that Amazon provides you to your domain in your domain `DNS` settings. The following is for Google Domains:

![ns](images/namesrvr.png)
![gd](images/gdns.png)

Go back to your instance networking settings and create a firewall rule so that we can do a quick connection test in a few steps

![int](images/inet.png)

`custom TCP on port 3000` - You can delete this later after your site is up

![poot](images/tcprule.png)

Go back to the homepage and click the terminal icon to `SSH` into your instance. Your instance should be up and running by now.

![ssh](images/ssh.png)

Go into htdocs

`cd htdocs`

Delete everything inside

`rm -rf *`

![dlt](images/dlt.png)

Clone this very repository!

`git clone https://github.com/joswayski/Lightsail-Setup-Custom-Domain-SSL.git`

Go into the repository folder

`cd Lightsail-Setup-Custom-Domain-SSL`

Install the dependencies

`npm install`

![installed](images/i.png)

Run the app to test it!

`node index.js`

![installed](images/i.png)

You should see: `Example app listening at http://localhost:3000`

If you go to your static IP from earlier and add `:3000` at the end, you should be able to see something like this:

![up](images/upnrunning.png)

Now we have to route HTTP traffic on `port 80` and all HTTPS traffic on `port 443` to our server's port, `3000`

> [Here](https://docs.bitnami.com/ibm/infrastructure/nodejs/administration/create-custom-application-nodejs/) is the documentation from Bitnami directly, or you can follow my steps below

Stop your Node app in the SSH terminal
`Control + C` on Mac, I believe it is the same for Windows.

In the terminal, type in `vi /opt/bitnami/apache/conf/vhosts/myapp-http-vhost.conf` and press enter

Type `i` for `INSERT` mode

Visit [this link](https://docs.bitnami.com/ibm/infrastructure/nodejs/administration/create-custom-application-nodejs/) and copy this code for port 80

> NOTE: If you would like to use another port in your app, make sure that you change the `http://localhost:3000` line to `http://localhost:YOUR_PORT_NUMBER` as well, in this section and the one coming up

![bitnami1](images/bitnami1.png)

### IMPORTANT:

- Replace _both_ instances of `/opt/bitnami/projects/myapp/public` with your app directory which, if you followed my tutorial, is in `/home/bitnami/htdocs/Lightsail-Setup-Custom-Domain-SSL`

![rme1](images/rme80.png)

Press your ESCAPE key

`esc`

And then force save

`:w!`

Quit the editor

`:q`

Now do the same thing for the other port:

`vi /opt/bitnami/apache/conf/vhosts/myapp-https-vhost.conf`

`ENTER`

`i` to enter `INSERT` mode

Copy and paste the second part for port 443

![bitnami2](images/bitnami2.png)

**Again**, be sure to replace the `/opt/bitnami/projects/myapp` with **your** app directory, or if you are following this tutorial: `/home/bitnami/htdocs/Lightsail-Setup-Custom-Domain-SSL`

![rm443](images/rme443.png)

Escape, force save, and quit the editor.

`esc`

`:w!`

`:q`

Restart the Apache server

`sudo /opt/bitnami/ctlscript.sh restart apache`

If you go back to your directory and run your app:

`cd /home/bitnami/htdocs/Lightsail-Setup-Custom-Domain-SSL`

`node index.js`

You will be able to visit your site without putitng the port number at the end!

![noport](images/noport.png)

To get your SSL certificate (fix that _Not Secure_ in your browser), run:

`sudo /opt/bitnami/bncert-tool`

> You will be prompted to enter this again, paste it in the terminal and run it again

![twicerun](images/twicerun.png)

You will then be prompted to enter the domain that you chose, enter `www.yourdomain.com` and `yourdomain.com`

![dom](images/dom.png)

- _Enable HTTP to HTTPS redirection [Y/n]:_ `Y`

- _Enable non-www to www redirection [Y/n]:_

> Up to you, I usually say `Y` because it looks more "professional"

- _Enable www to non-www redirection [Y/n]:_ `N`

> Again, personal preference. I say `N` to this

- _Do you agree to these changes? [Y/n]:_ `Y`

Enter your email and hit `Y` on the following prompts. This will take a minute.

![donezo](images/emial.png)

Go back to our app and run the server forever!

`cd /home/bitnami/htdocs/Lightsail-Setup-Custom-Domain-SSL`

`forever start index.js`

Your server is up and running on your custom domain with SSL! Congrats!

![fin](images/fin.png)

---

Feel free to delete the firewall rule on port 3000 we created earlier. Do not hesitate for any PR's or issues. Happy hosting!
