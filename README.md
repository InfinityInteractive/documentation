# SSL Certificates ( Cloudflare API method)

### Setting Up SSL Certificates with acme.sh and CloudFlare API

This guide is intended for advanced users whose server systems do not have access to port 80 for traditional SSL certificate issuance. We'll use acme.sh, a powerful shell script for managing Let's Encrypt certificates, in conjunction with CloudFlare's API for DNS verification. If you use a different DNS provider, you can research acme.sh's documentation for alternative configurations.

#### Installing acme.sh

1.  To get started, you need to install acme.sh. Run the following command on your Ubuntu-based distribution:

    ```bash
     codecurl https://get.acme.sh | sh
    ```

#### Obtaining CloudFlare API Key

2. After installing acme.sh, you'll need to obtain a CloudFlare API key. Follow these steps:
   * Visit the CloudFlare website and select your domain.
   * On the right side, copy your "Zone ID" and "Account ID."
   * Click on "Get your API token" and then "Create Token."
   * Select the template "Edit zone DNS" and set the scope to "Zone Resources."
   * Click "Continue to summary" and copy the generated API token.

#### Creating a Certificate Folder

3.  Since acme.sh's configuration is based on Certbot, you'll need to create the required folder manually:

    ```bash
     codesudo mkdir -p /etc/letsencrypt/live/example.com
    ```

    Replace "example.com" with your actual domain.

#### Configuring acme.sh

4.  Set your CloudFlare API credentials as environment variables. Replace the placeholders with your actual values.

    ```bash
     codeexport CF_Token="Your_CloudFlare_API_Key"
    export CF_Account_ID="Your_CloudFlare_Account_ID"
    export CF_Zone_ID="Your_CloudFlare_Zone_ID"
    ```
5.  Now, create the SSL certificate using acme.sh, specifying your domain and the key and fullchain file locations:

    ```bash
     codeacme.sh --issue --dns dns_cf -d "example.com" --server letsencrypt \
    --key-file /etc/letsencrypt/live/example.com/privkey.pem \
    --fullchain-file /etc/letsencrypt/live/example.com/fullchain.pem
    ```

    Make sure to replace "example.com" with your actual domain.

#### Auto Renewal

6.  After running the script for the first time, it will be added to the crontab for automated certificate renewal. You can edit the auto-renewal interval by modifying the crontab:

    ```bash
     codesudo crontab -e
    ```

    Adjust the cron schedule as needed to ensure your SSL certificates are automatically renewed at your preferred intervals.

With these steps, you have successfully set up SSL certificates using acme.sh and CloudFlare's DNS verification. This advanced approach is useful when port 80 access is restricted or unavailable.
