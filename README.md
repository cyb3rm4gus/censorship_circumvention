# Unrestricted Internet Access 101

### Relevant links
[3x-ui github repository](https://github.com/MHSanaei/3x-ui) - easily installable & highly customizable stealthy proxy server with web-based control panel. Uses [xray](https://github.com/XTLS/Xray-core) as the proxy backend.
[Hysteria2 project website](https://v2.hysteria.network/) - stealthy proxy server software.
[NekoBox client software](https://getnekobox.com/en/) - Windows, Linux & Android app compatible with a range of stealthy proxy technologies.
[AmneziaWG](https://amnezia.org/) - stealthy VPN, a fork of [wireguard](https://www.wireguard.com/) with added obfuscation & traffic mimicry to QUIC.
[Streisand](https://apps.apple.com/us/app/streisand/id6450534064) - iOS client app compatible with a range of stealthy proxy technologies.

## Setting up 3x-ui on a VPS

### Step 1: get a VPS
To set up a private proxy server you need a Virtual Private Server (VPS). Below is a list of recommended hosting providers to buy a VPS from:
- [DigitalOcean](https://m.do.co/c/2361edd88d04)
- [Linode](linode.com)
- [Hostinger](https://hostinger.com?REFERRALCODE=SNQCYBERSKZF)
### Step 2: 3x-ui installation

Not required, but recommended step: update server software to stay secure.
```bash
apt update && apt upgrade -y
```

Install 3x-ui: copy this command and execute on the server
```bash
bash <(curl -Ls https://raw.githubusercontent.com/mhsanaei/3x-ui/master/install.sh)
```

When prompted if you want to set up a port manually, preferably say **no** if you don't want to have any specific port to be used for web panel access. It's not a proxy port, port for the actual proxy to work will be configured later in the web panel.

Upon completion script will show you: login, password, web base path & link. **Save that info**, this is the only time you will be shown login & password. You can change them later, if necessary.

### Step 3: switching panel to HTTPS

I do not recommend you to log into the panel using initial setup, because by default it uses HTTP with NO encryption, and you will leak your login, password and web base path to ISP and any other network node between your machine and the server.

You can create & add to the panel settings a self-signed SSL cert using this command:
```bash
bash <(curl -Ls https://raw.githubusercontent.com/cyb3rm4gus/3x-ui-auto_add_ssl/refs/heads/main/3x-ui-autossl.sh)
```

After the script finishes to execute, you will see a message that SQL injects were successful. After that you **need to restart the panel**.

Run command:
```bash
x-ui
```

And in the menu select the option **restart**, it's number 13 as of the moment of this manual being created. After that select option **show current settings** (number 10) and check that you see a message that panel is secure with SSL.

Copy the link from the script and paste it into your browser. You will see a security warning, telling you that **connection is not secure**. What it means is that your connection is encrypted by newly-created self-signed SSL certificate, but browser can't verify it's authenticity, because it's self-signed.

Select **Advanced** and then **Accept risk & continue**.
### Step 4: creating a proxy configuration
Log in with your credentials shown after the installation of 3x-ui.

Proceed to **Inbounds** in the left menu, and click **Add inbound**
*For a step-by-step setup please watch the [video](https://youtu.be/7GtUVT0b1qs?si=dQrEhWXcIHmrlgEe&t=1197)* *If something is not mentioned below, don't touch it, leave as it is*

- Protocol: vless
- Port: 443
- Security: Reality
- uTLS: chrome
- Target: a website that is not censored in your location, pick some website that usually has a lot of traffic (video streaming platforms, social networks etc.)
- Under **Private key** click on **Get new cert**
- Click **Create**
### Step 5: flow setup
Security flow needs to be applied to the client config. Click on the plus icon on the newly created Inbound, then **edit** (icon with pen) and in **flow** select **xtls-rprx-vision**
### Step 6: connecting clients
There are two ways to add proxy configuration to a client app:
- QR code for mobile apps, to see it click on the QR icon in the client config line in **Inbounds**
- URL for desktop clients, to get it click in **more info** (i) in the client config line, scroll down and you will see the URL
