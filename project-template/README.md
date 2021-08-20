# lando-template

...

## Vývojové prostředí

Spuštění aplikace:

    lando start

Import Certifikační autority `~/.lando/certs/lndo.site.crt`:

- Operační systém ([Add trusted root certificates](https://manuals.gfi.com/en/kerio/connect/content/server-configuration/ssl-certificates/adding-trusted-root-certificates-to-the-server-1605.html)):
  - Linux

        sudo cp ~/.lando/certs/lndo.site.crt /usr/local/share/ca-certificates/lndo.site.crt
        sudo update-ca-certificates

  - Windows

        certutil -addstore -f "ROOT" ~/.lando/certs/lndo.site.crt       

  - MacOS

        sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain ~/.lando/certs/lndo.site.crt

- Chrome:
  - Customize and control Chromium -> Settings -> Privacy & Security -> Security -> Manage Certificates -> Authorities -> Import:
    - Certificate authority:
      - Trust this certificate for identifying websites: **Yes** 
- Firefox:
  - Aplication Menu -> Settings -> Privacy & Security -> Security -> Certificates -> View Certificates -> Authorities -> Import:
      - Trust settings:
        - This certificate can identify websites: **Yes**

## Tipy

Odkazy:
- [Accessing Your Services Externally](https://docs.lando.dev/guides/external-access.html#locking-down-ports)