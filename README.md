
# Signal-Installation-Tour
Finally, after one month from zero, I succeed in building the Signal on premise system and custom it as I want.

![https://www199.lunapic.com/do-not-link-here-use-hosting-instead/156756811786810934?2863527667](https://www199.lunapic.com/do-not-link-here-use-hosting-instead/156756811786810934?2863527667)
# 1. Signal Server
	a. First you must setup CDS at step 2 and run it
	b. Signal service config file
	https://github.com/iamdatvt94/Signal-Installation-Tour/blob/master/signal-service-config.yml
		- Twilio: you must create twilio account and use it if you want to send sms
		- All redis config: you should setup 3 nodes cluster
		- CDS replication endpoint base url:  https url of CDS. replicationPassword you can set anything and replicationCaCertificate you must get it from https cds url
		- attachments and profiles: you need create s3 bucket in amazon
		- database, messageStore and abuseDatabase: you must set up Postgresql and create three database: accountdb, messagedb, abusedb and grant roles for account in config.
		-> you must run config to initial Signal db:
		~~java -jar ../jars/Signal-version.jar  accountdb migrate  ../config/config.yml~~
		~~java -jar ../jars/Signal-version.jar  messagedb migrate  ../config/config.yml~~
		~~java -jar ../jars/Signal-version.jar  abusedb migrate  ../config/config.yml~~
		- Now, just start it by command java -jar ..... and check logs

# 2. Signal Contact Discovery Service
    a. **Intel SGX**
        - Link github: https://github.com/intel/linux-sgx.git
        - Before install SGX, you need to check your hardware, it need to support SGX2 by the tool:
        https://github.com/ayeks/SGX-hardware
        - You can use *MS Azure cloud*, Ubuntu 18.04, it supports SGX2 and now , I'm using it for my product.
        - If your hardware support SGX, install Intel SGX by steps:
            . Install SGX Driver: https://github.com/intel/linux-sgx-driver (Note: check out branch sgx2)
            . Install SGX PSW
            . ECDSA attestation and SGX SDK you do not need to.
    b. **CDS installation**
        - Git clone https://github.com/signalapp/ContactDiscoveryService.git
        - make -C enclave (Or you can run with sudo)
        - The most important thing is change .yml config file
        # Config CDS file
        https://github.com/iamdatvt94/Signal-Installation-Tour/blob/master/CDS-config.yml
        - You need to subscribe Intel® SGX Attestation Service Utilizing Enhanced Privacy ID
        https://api.portal.trustedservices.intel.com/
         -> go to Development Access (unlinkable)
         -> you can get spid and iasSecretKey
        - *mrenclave*:
            . after run command make -C enclave. you cd to folder: service/src/main/resource/enclave/ and can get *.so file
            -> file's name is mrenclave value
        - *redis config*: you setup redis (cluster 3 nodes) and setup sentinel 
            Ex: https://www.inovex.de/blog/redis-sentinel-make-your-dataset-highly-available/
        - *sqs*:
            . Set up you sqs in amazon , note it must be fifo type
        - Start CDS service
            . CDS will running on port 8000
            . Signal service only using CDS with https url
            . Your must be set url to https and get it's certificate for using in Signal service and Client (android, ios,...)
# 3. Signal Android


