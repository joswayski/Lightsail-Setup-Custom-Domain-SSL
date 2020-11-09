# Deploy a Node / Express app on AWS Lightsail with a Custom Domain + SSL

-------------> [Video Tutorial (_10 min_)](https://www.youtube.com/watch?v=rtshCulV2hk) <-------------

### Steps below if you prefer images

- Create a Lightsail instance from the **[AWS dashboard](https://lightsail.aws.amazon.com/ls/webapp/home/instances)**

![Dashboard Image](images/dash1.png)

- Choose your instance type

**_Linux & Node_**
![Instance type](images/instanceType.png)

- Wait for the instance to be created
  ![Wait](images/waiting.png)

- In the meantime, go to the **_Networking_** tab and create a static IP
  ![Static](images/static.png)

- Attach the static IP you just created to your instance

![attachIP](images/attachIP.png)

- Go back [home](https://lightsail.aws.amazon.com/ls/webapp/home/instances) and click on [networking](https://lightsail.aws.amazon.com/ls/webapp/home/networking) so that you arrive at this tab

![networktab](images/networking.png)

- Click on **_Create DNS zone_** and enter your domain here

![domain](images/domain.png)

> I personally use [Google Domains](https://domains.google.com/registrar/) to buy my domains but if you prefer to stay in the ecosystem, Amazon has their own solution called [Route53](https://aws.amazon.com/route53/)
