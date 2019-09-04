# Signal-Installation-Tour
Finally, I have successfully built the Signal.

# 1. Signal Server
# 2. Signal Contact Discovery Service
    a. Intel SGX
        - Link github: https://github.com/intel/linux-sgx.git
        - Before install SGX, you need to check your hardware, it need to support SGX2 by the tool:
        https://github.com/ayeks/SGX-hardware
        - You can use MS Azure cloud, Ubuntu 18.04, it supports SGX2 and now , I'm using it for my product.
        - If your hardware support SGX, install Intel SGX by steps:
            . Install SGX Driver: https://github.com/intel/linux-sgx-driver (Note: check out branch sgx2)
            . Install SGX PSW
            . ECDSA attestation and SGX SDK you do not need to.
    b. CDS installation
        - Git clone https://github.com/signalapp/ContactDiscoveryService.git
        - make -C enclave (Or you can run with sudo)
        - The most important thing is change .yml config file
        # Config CDS file
        
# 3. Signal Android


