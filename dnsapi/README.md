# How to use DNS API

## 1. Use CloudFlare domain API to automatically issue cert

First you need to login to your CloudFlare account to get your API key.

```
export CF_Key="sdfsdfsdfljlbjkljlkjsdfoiwje"
export CF_Email="xxxx@sss.com"
```

Ok, let's issue a cert now:
```
acme.sh --issue --dns dns_cf -d example.com -d www.example.com
```

The `CF_Key` and `CF_Email` will be saved in `~/.acme.sh/account.conf` and will be reused when needed.


## 2. Use DNSPod.cn domain API to automatically issue cert

First you need to login to your DNSPod account to get your API Key and ID.

```
export DP_Id="1234"
export DP_Key="sADDsdasdgdsf"
```

Ok, let's issue a cert now:
```
acme.sh --issue --dns dns_dp -d example.com -d www.example.com
```

The `DP_Id` and `DP_Key` will be saved in `~/.acme.sh/account.conf` and will be reused when needed.


## 3. Use CloudXNS.com domain API to automatically issue cert

First you need to login to your CloudXNS account to get your API Key and Secret.

```
export CX_Key="1234"
export CX_Secret="sADDsdasdgdsf"
```

Ok, let's issue a cert now:
```
acme.sh --issue --dns dns_cx -d example.com -d www.example.com
```

The `CX_Key` and `CX_Secret` will be saved in `~/.acme.sh/account.conf` and will be reused when needed.


## 4. Use GoDaddy.com domain API to automatically issue cert

First you need to login to your GoDaddy account to get your API Key and Secret.

https://developer.godaddy.com/keys/

Please create a Production key, instead of a Test key.

```
export GD_Key="sdfsdfsdfljlbjkljlkjsdfoiwje"
export GD_Secret="asdfsdafdsfdsfdsfdsfdsafd"
```

Ok, let's issue a cert now:
```
acme.sh --issue --dns dns_gd -d example.com -d www.example.com
```

The `GD_Key` and `GD_Secret` will be saved in `~/.acme.sh/account.conf` and will be reused when needed.


## 5. Use PowerDNS embedded API to automatically issue cert

First you need to login to your PowerDNS account to enable the API and set your API-Token in the configuration.

https://doc.powerdns.com/md/httpapi/README/

```
export PDNS_Url="http://ns.example.com:8081"
export PDNS_ServerId="localhost"
export PDNS_Token="0123456789ABCDEF"
export PDNS_Ttl=60
```

Ok, let's issue a cert now:
```
acme.sh --issue --dns dns_pdns -d example.com -d www.example.com
```

The `PDNS_Url`, `PDNS_ServerId`, `PDNS_Token` and `PDNS_Ttl` will be saved in `~/.acme.sh/account.conf` and will be reused when needed.


## 6. Use OVH/kimsufi/soyoustart/runabove API to automatically issue cert

https://github.com/Neilpang/acme.sh/wiki/How-to-use-OVH-domain-api


## 7. Use nsupdate to automatically issue cert

First, generate a key for updating the zone
```
b=$(dnssec-keygen -a hmac-sha512 -b 512 -n USER -K /tmp foo)
cat > /etc/named/keys/update.key <<EOF
key "update" {
    algorithm hmac-sha512;
    secret "$(awk '/^Key/{print $2}' /tmp/$b.private)";
};
EOF
rm -f /tmp/$b.{private,key}
```

Include this key in your named configuration
```
include "/etc/named/keys/update.key";
```

Next, configure your zone to allow dynamic updates.

Depending on your named version, use either
```
zone "example.com" {
    type master;
    allow-update { key "update"; };
};
```
or
```
zone "example.com" {
    type master;
    update-policy {
        grant update subdomain example.com.;
    };
}
```

Finally, make the DNS server and update Key available to `acme.sh`

```
export NSUPDATE_SERVER="dns.example.com"
export NSUPDATE_KEY="aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa=="
```

Ok, let's issue a cert now:
```
acme.sh --issue --dns dns_nsupdate -d example.com -d www.example.com
```

The `NSUPDATE_SERVER` and `NSUPDATE_KEY` settings will be saved in `~/.acme.sh/account.conf` and will be reused when needed.


## 8. Use LuaDNS domain API

Get your API token at https://api.luadns.com/settings

```
export LUA_Key="sdfsdfsdfljlbjkljlkjsdfoiwje"
export LUA_Email="xxxx@sss.com"
```

To issue a cert:
```
acme.sh --issue --dns dns_lua -d example.com -d www.example.com
```

The `LUA_Key` and `LUA_Email` will be saved in `~/.acme.sh/account.conf` and will be reused when needed.


## 9. Use DNSMadeEasy domain API

Get your API credentials at https://cp.dnsmadeeasy.com/account/info

```
export ME_Key="sdfsdfsdfljlbjkljlkjsdfoiwje"
export ME_Secret="qdfqsdfkjdskfj"
```

To issue a cert:
```
acme.sh --issue --dns dns_me -d example.com -d www.example.com
```

The `ME_Key` and `ME_Secret` will be saved in `~/.acme.sh/account.conf` and will be reused when needed.


## 10. Use Amazon Route53 domain API

https://github.com/Neilpang/acme.sh/wiki/How-to-use-Amazon-Route53-API

```
export  AWS_ACCESS_KEY_ID=XXXXXXXXXX
export  AWS_SECRET_ACCESS_KEY=XXXXXXXXXXXXXXX
```

To issue a cert:
```
acme.sh --issue --dns dns_aws -d example.com -d www.example.com
```

The `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` will be saved in `~/.acme.sh/account.conf` and will be reused when needed.

## 11. Use Aliyun domain API to automatically issue cert

First you need to login to your Aliyun account to get your API key.
[https://ak-console.aliyun.com/#/accesskey](https://ak-console.aliyun.com/#/accesskey)

```
export Ali_Key="sdfsdfsdfljlbjkljlkjsdfoiwje"
export Ali_Secret="jlsdflanljkljlfdsaklkjflsa"
```

Ok, let's issue a cert now:
```
acme.sh --issue --dns dns_ali -d example.com -d www.example.com
```

The `Ali_Key` and `Ali_Secret` will be saved in `~/.acme.sh/account.conf` and will be reused when needed.

## 12. Use ISPConfig 3.1 API

This only works for ISPConfig 3.1 (and newer).

Create a Remote User in the ISPConfig Control Panel. The Remote User must have access to at least `DNS zone functions` and `DNS txt functions`.

```
export ISPC_User="xxx"
export ISPC_Password="xxx"
export ISPC_Api="https://ispc.domain.tld:8080/remote/json.php"
export ISPC_Api_Insecure=1
```
If you have installed ISPConfig on a different port, then alter the 8080 accordingly.
Leaver ISPC_Api_Insecure set to 1 if you have not a valid ssl cert for your installation. Change it to 0 if you have a valid ssl cert.

To issue a cert:
```
acme.sh --issue --dns dns_ispconfig -d example.com -d www.example.com
```

The `ISPC_User`, `ISPC_Password`, `ISPC_Api`and `ISPC_Api_Insecure` will be saved in `~/.acme.sh/account.conf` and will be reused when needed.

## 13. Use Alwaysdata domain API

First you need to login to your Alwaysdata account to get your API Key.

```sh
export AD_API_KEY="myalwaysdataapikey"
```

Ok, let's issue a cert now:

```sh
acme.sh --issue --dns dns_ad -d example.com -d www.example.com
```

The `AD_API_KEY` will be saved in `~/.acme.sh/account.conf` and will be reused
when needed.

## 14. Use Linode domain API

First you need to login to your Linode account to get your API Key.
[https://manager.linode.com/profile/api](https://manager.linode.com/profile/api)

Then add an API key with label *ACME* and copy the new key.

```sh
export LINODE_API_KEY="..."
```

Due to the reload time of any changes in the DNS records, we have to use the `dnssleep` option to wait at least 15 minutes for the changes to take effect.

Ok, let's issue a cert now:

```sh
acme.sh --issue --dns dns_linode --dnssleep 900 -d example.com -d www.example.com
```

The `LINODE_API_KEY` will be saved in `~/.acme.sh/account.conf` and will be reused when needed.

## 15. Use FreeDNS

FreeDNS (https://freedns.afraid.org/) does not provide an API to update DNS records (other than IPv4 and IPv6
dynamic DNS addresses).  The acme.sh plugin therefore retrieves and updates domain TXT records by logging
into the FreeDNS website to read the HTML and posting updates as HTTP.  The plugin needs to know your
userid and password for the FreeDNS website.

```sh
export FREEDNS_User="..."
export FREEDNS_Password="..."
```

You need only provide this the first time you run the acme.sh client with FreeDNS validation and then again
whenever you change your password at the FreeDNS site.  The acme.sh FreeDNS plugin does not store your userid
or password but rather saves an authentication token returned by FreeDNS in `~/.acme.sh/account.conf` and
reuses that when needed.

Now you can issue a certificate.

```sh
acme.sh --issue --dns dns_freedns -d example.com -d www.example.com
```

Note that you cannot use acme.sh automatic DNS validation for FreeDNS public domains or for a subdomain that
you create under a FreeDNS public domain.  You must own the top level domain in order to automaitcally
validate with acme.sh at FreeDNS.

## 16. Use cyon.ch

You only need to set your cyon.ch login credentials.
If you also have 2 Factor Authentication (OTP) enabled, you need to set your secret token too and have `oathtool` installed.

```
export CY_Username="your_cyon_username"
export CY_Password="your_cyon_password"
export CY_OTP_Secret="your_otp_secret" # Only required if using 2FA
```

To issue a cert:
```
acme.sh --issue --dns dns_cyon -d example.com -d www.example.com
```

The `CY_Username`, `CY_Password` and `CY_OTP_Secret` will be saved in `~/.acme.sh/account.conf` and will be reused when needed.

## 17. Use Domain-Offensive/Resellerinterface/Domainrobot API

You will need your login credentials (Partner ID+Password) to the Resellerinterface, and export them before you run `acme.sh`:
```
export DO_PID="KD-1234567"
export DO_PW="cdfkjl3n2"
```

Ok, let's issue a cert now:
```
acme.sh --issue --dns dns_do -d example.com -d www.example.com
```

## 18. Use Gandi LiveDNS API

You must enable the new Gandi LiveDNS API first and the create your api key, See: http://doc.livedns.gandi.net/

```
export GANDI_LIVEDNS_KEY="fdmlfsdklmfdkmqsdfk"
```

Ok, let's issue a cert now:
```
acme.sh --issue --dns dns_gandi_livedns -d example.com -d www.example.com
```

## 19. Use Knot (knsupdate) DNS API to automatically issue cert

First, generate a TSIG key for updating the zone.

```
keymgr tsig generate acme_key algorithm hmac-sha512 > /etc/knot/acme.key
```

Include this key in your knot configuration file.

```
include: /etc/knot/acme.key
```

Next, configure your zone to allow dynamic updates.

Dynamic updates for the zone are allowed via proper ACL rule with the `update` action. For in-depth instructions, please see [Knot DNS's documentation](https://www.knot-dns.cz/documentation/).

```
acl:
  - id: acme_acl
    address: 192.168.1.0/24
    key: acme_key
    action: update

zone:
  - domain: example.com
    file: example.com.zone
    acl: acme_acl
```

Finally, make the DNS server and TSIG Key available to `acme.sh`

```
export KNOT_SERVER="dns.example.com"
export KNOT_KEY=`grep \# /etc/knot/acme.key | cut -d' ' -f2`
```

Ok, let's issue a cert now:
```
acme.sh --issue --dns dns_knot -d example.com -d www.example.com
```

The `KNOT_SERVER` and `KNOT_KEY` settings will be saved in `~/.acme.sh/account.conf` and will be reused when needed.

# Use custom API

If your API is not supported yet, you can write your own DNS API.

Let's assume you want to name it 'myapi':

1. Create a bash script named `~/.acme.sh/dns_myapi.sh`,
2. In the script you must have a function named `dns_myapi_add()` which will be called by acme.sh to add the DNS records.
3. Then you can use your API to issue cert like this:

```
acme.sh --issue --dns dns_myapi -d example.com -d www.example.com
```

For more details, please check our sample script: [dns_myapi.sh](dns_myapi.sh)


# Use lexicon DNS API

https://github.com/Neilpang/acme.sh/wiki/How-to-use-lexicon-dns-api
