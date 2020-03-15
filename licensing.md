# Licensing

**Universal Automation** is licensed per agent. Whenever you run `Start-UAServer`, you are starting a new agent. You can operate Universal Automation without a license however there will be a number of restrictions in place. The Universal Automation dashboard also requires a license to Universal Dashboard. 

**Universal Automation Desktop** is licensed per user. You can install it on machines where you are the primary user. 

## Restrictions on unlicensed Universal Automation

- Run up to 2 jobs concurrently  
- Maximum 25 jobs per day

## Obtaining a Universal Automation License

You can purchase a license for your agent from [IronmanSoftware.com](https://ironmansoftware.com/universal-automation/).

Licenses require internet access for activation. They will attempt to communicate with IronmanSoftware.com. If you require an offline license key, please [contact us](http://ironmansoftware.com/contact-us). 

## Installing a License

You can install a license from the licensing tab. 

![License Dialog for Universal Automation](.gitbook/assets/image%20%2810%29.png)

## Installing a License from the command line

Your UA server needs to be running to install a license. 

```text
Set-UALicense -Key 'universalautomation-asdfasdf2309yafk' -ComputerName http://localhost:1000
```

