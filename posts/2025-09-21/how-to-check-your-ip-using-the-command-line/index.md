<!--
.. title: How to check your IP using the command line?
.. slug: how-to-check-your-ip-using-the-command-line
.. date: 2025-09-21 12:19:39 UTC+02:00
.. tags: network,curl,IP
.. category: network
.. link: 
.. description: 
.. type: text
-->

You just need to fetch `ipinfo.io`, either using `curl`:
```shell
curl ipinfo.io
```
or `wget`:
```shell
wget -q -O - ipinfo.io
```

Or if you want to get just the IP (IPv6 is preferred):
```shell

curl ifconfig.me

```
or get explicitly IPv4

```shell

curl -4 ifconfig.me
```


## More of ifconfig.me

### Command Line Examples
```bash
# Basic usage - returns your IP (IPv4 or IPv6, depending on your connection)
curl ifconfig.me

# Force IPv4
curl -4 ifconfig.me
curl ipv4.ifconfig.me

# Force IPv6
curl -6 ifconfig.me
curl ipv6.ifconfig.me

# Using wget instead
wget -qO- ifconfig.me
```

## Additional Features

ifconfig.me offers more than just your IP address. You can retrieve various information about your connection:

```bash
# Get all information
curl ifconfig.me/all

# Get specific headers
curl ifconfig.me/host          # Hostname
curl ifconfig.me/ua            # User agent
curl ifconfig.me/port          # Port number
curl ifconfig.me/forwarded     # X-Forwarded-For header
```

## IPv6 vs IPv4: Why You Might See an IPv6 Address

If ifconfig.me shows you an IPv6 address (something like `2001:0db8:85a3::8a2e:0370:7334`), it's because:

1. **Your ISP provides IPv6** - Most modern ISPs now offer IPv6 connectivity alongside traditional IPv4.

2. **Your system prefers IPv6** - When both IPv4 and IPv6 are available, modern operating systems typically prefer IPv6 by default.

3. **ifconfig.me supports dual-stack** - The service has both IPv4 and IPv6 addresses, so it responds to whichever protocol you use to connect.

This is actually a positive sign—IPv6 is the future of internet addressing, designed to solve the IPv4 address exhaustion problem.

## Similar Services

While ifconfig.me is popular, several alternatives exist:

- `icanhazip.com` - Another simple IP lookup service
- `ipinfo.io` - Provides additional geolocation data
- `ifconfig.co` - Offers more detailed JSON output
- `ident.me` - Minimal service similar to ifconfig.me
- `checkip.amazonaws.com` - Amazon's IP checking service
