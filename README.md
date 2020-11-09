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

Click on **_Create DNS zone_** and enter your domain here

![domain](images/domain.png)

> I personally use [Google Domains](https://domains.google.com/registrar/) to buy my domains but if you prefer to stay in the ecosystem, Amazon has their own solution called [Route53](https://aws.amazon.com/route53/)

> I will be using `opensourceme.app` for the rest of this tutorial as I have this one for testing

Click `create` and add two `A` records

The first one should be `@` in the first box on the left and `Resolves to` should be the static IP address we created a few steps ago
![rs](images/arcrd1.png)

Your second one should be `www` in the first box and once more, select your static IP

![added](images/addedARecords.png)

Add the name servers that Amazon provides you to your domain in your registrar's settings. The following is for Google Domains:

![ns](images/namesrvr.png)
![gd](images/gdns.png)
