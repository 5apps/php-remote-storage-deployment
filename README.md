# Deployment on VM/Server
We assume you have a clean installation of Fedora 23. We tested it with the
OpenStack image available [here](https://getfedora.org/en/cloud/download/).

    $ curl -L -O https://github.com/fkooman/php-remote-storage-deployment/archive/master.tar.gz
    $ tar -xzf master.tar.gz
    $ cd php-remote-storage-deployment-master

Now you can modify the `deploy.sh` script to change the `HOSTNAME` variable to
your own name of choice, e.g.:

    HOSTNAME=storage.tuxed.net

Now, run the script:
    
    $ sh deploy.sh

This should set everything up, including a working (self-signed) TLS 
certificate.

## Obtaining a CA Signed Certificate
The script will output a CSR (certificate signing request) in the directory
where you run `deploy.sh` that can be sent to a CA of choice. In this 
example case it would be `storage.tuxed.net.csr`. 

Once you obtain a certificate from your CA you can overwrite 
`/etc/pki/tls/certs/storage.tuxed.net.crt`. Do not forget to also place the 
certificate chain you obtained from the CA in 
`/etc/pki/tls/certs/storage.tuxed.net-chain.crt`, and enable the chain in 
`/etc/httpd/conf.d/storage.tuxed.net.conf`:

    SSLCertificateChainFile /etc/pki/tls/certs/storage.tuxed.net-chain.crt

Now restart Apache and you should be fully up and running!

    $ sudo systemctl restart httpd
