# Licensing

Universal Automation is licensed per agent. Whenever you run `Start-UAServer`, you are starting a new agent. You can purchase a license for your agent from [IronmanSoftware.com](https://ironmansoftware.com/universal-automation/). 

Licenses require internet access for activation. They will attempt to communicate with IronmanSoftware.com. If you require an offline license key, please [contact us](http://ironmansoftware.com/contact-us). 

## Installing a License

Your UA server needs to be running to install a license. 

```text
Set-UALicense -Key 'universalautomation-asdfasdf2309yafk' -ComputerName http://localhost:1000
```

