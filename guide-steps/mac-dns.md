# Title #
Setup DNS-level ad-blocking on macOS

# Summary #

In this guide you'll learn how to manually insert DNS entries for known malicious hosts (e.g. ad-servers, trackers,
malware websites) and point them to an empty address, so that those requests are blocked on your device. Unlike browser
add-ons, DNS-level ad-blocking works on *any* application or service running on your device.

# Body #

### Setup ###

In the Internet, requests are routed to IP addresses. Since IP addresses are hard to remember, we usually address hosts
by their host-name (e.g privacyinternational.org). As such, and because IP addresses can change frequently, when your
computer wants to access a server by its host-name, it asks a DNS server what the IP address for that host-name is, so
that it can route the request. Typically, your operating systems first checks your system's *hosts file* for an address
to the host-name. If the host-name is not present in the file the operating system asks an external DNS server to
resolve it.

To setup DNS-level ad-blocking, we will add a list of known malicious ad-servers and trackers to the hosts file and
point them to an empty address (`0.0.0.0`), thus ensuring the requests are blocked. The lists of malicious hosts are
provided and maintained by the online community, and you can pick several lists to block different types of services
(e.g. ads, trackers, fake news, social media, etc.). In this guide, we suggest you use the [unified hosts
list](https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts) from Steven Black to block ads and trackers.

Start by opening a Finder window and then navigate to the **Applications > Utilities** folder and open the **Terminal**
application. Then, type the following command into the prompt:

```bash
wget https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts -O hosts
```

and press Enter. The file will be downloaded to your current directory. Although unlikely, it's important to inspect the
contents of the file, to ensure it does not contain malicious entries (e.g. paypal.com pointing to a phishing address
that steals your credit card information). To do so, you can use `grep`. In the same terminal window, type the following
command

```bash
grep -E '^(\s*)[1-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\s*.*\..*' hosts
```

and then press Enter. If there's any output, the file might have been tampered with. Delete it and choose another from
the provided lists.

If the hosts file is OK, you can move it to the correct location. On macOS, that's typically `/etc/hosts`. Please note
that if you have custom entries on your hosts file, you need to manually merge both files (open both files on your text
editor and copy-paste the appropriate contents). In the same terminal window, type the following command.

```bash
sudo mv hosts /etc
```

Click Enter and you will be prompted to enter your password. Write your password in the prompt (it will not show up as
you type, and that's expected) and click Enter again. For the changes to have effect, you need to either restart your
network service, or reboot the machine.

From now on, your operating system will block any request to ad-servers and trackers contained in your hosts file. To
keep the hosts file up to date, periodically repeat the steps in this guide.