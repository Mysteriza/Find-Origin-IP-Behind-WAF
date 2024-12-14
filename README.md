# Origin IP Discovery Behind WAF

This guide provides various methods to discover the original IP address of a website behind a Web Application Firewall (WAF). Each method includes tools and techniques to identify potential IPs while ensuring WAF is bypassed.

- I summarize it from: [Mastering Origin IP Discovery Behind WAF | 11+ method](https://youtu.be/R3hmZpkvCmc?si=1a-ZsnKlYqj406Oz)
- And his website: [LostSec | Coffinxp](https://lostsec.xyz/)
- Github: [coffinxp](https://github.com/coffinxp)

# What is this for?

This guide is designed to assist penetration testers and cybersecurity professionals in identifying the original IP address of a website protected by a WAF. With the original IP address, testers can:

- Conduct direct vulnerability assessments on the server without WAF interference.
- Map the network to better understand the target's infrastructure.
- Detect any misconfigurations or policy errors on the main server.

These methods must be used ethically and only on systems you have permission to test.

## Method 1: Using dnsrecon
- **Command**:
  ```bash
  python3 dnsrecon.py -d target.com
  ```
- Check if the original IP of the target website is revealed.
- Verify no WAF is detected on the identified IP using:
  - **Wappalyzer extension**.
  - **wafw00f tool**.

## Method 2: Using Shodan Dork
- Login to [shodan.io](https://www.shodan.io)
- **Query**
  ```
  ssl.cert.subject.CN:'example.com' 200
  ```
- Inspect results for possible original IPs pointing to the website without errors.
- Verify no WAF is detected using:
  - **Wappalyzer extension**.
  - **wafw00f tool**.

## Method 3: Using Favicon Finder
1. Visit [favicons.teamtailor-cdn.com](https://favicons.teamtailor-cdn.com).
2. Input the target website URL to locate its `favicon.ico`.
3. Verify if the favicon link redirects to the target website. Then copy the URL.
4. Paste the URL to [favicon-hash.kmsec.uk](https://favicon-hash.kmsec.uk) to generate a hash of the favicon.
5. Use the "Search Sensys" link to find possible original IPs.
6. Check results one by one.
- Verify no WAF is detected using:
  - **Wappalyzer extension**.
  - **wafw00f tool**.

## Method 4: Using IP History
1. Visit [viewdns.info](https://viewdns.info/).
2. Enter the target website URL in the **IP History** section.
3. Examine the IP history for potential original IPs.
- Verify no WAF is detected using:
  - **Wappalyzer extension**.
  - **wafw00f tool**.

## Method 5: Using SPF Record
1. Visit [mxtoolbox.com](https://mxtoolbox.com/).
2. Search for the SPF record of the target website.
3. Inspect the results for any original IPs.
- Verify no WAF is detected using:
  - **Wappalyzer extension**.
  - **wafw00f tool**.

## Method 6: Using SecurityTrails
1. Visit [securitytrails.com](https://securitytrails.com/).
2. Search the target website and navigate to the **Historical Data** section.
3. Identify any potential original IPs.
- Verify no WAF is detected using:
  - **Wappalyzer extension**.
  - **wafw00f tool**.

## Method 7: Using Censys
1. Visit [search.censys.io](https://search.censys.io/).
2. Search the target website and filter results using **Product Version**.
3. Check for potential original IPs.
- Verify no WAF is detected using:
  - **Wappalyzer extension**.
  - **wafw00f tool**.

## Method 8: Using Fofa
1. Visit [fofa.info](https://en.fofa.info/).
2. Search the target website, filtering results with **Favicon**.
3. Examine the results for possible original IPs.
- Verify no WAF is detected using:
  - **Wappalyzer extension**.
  - **wafw00f tool**.

## Method 9: Using ZoomEye
1. Visit [zoomeye.hk](https://www.zoomeye.hk/).
2. Search the target website and filter results using **Favicon** or **IPv4**.
3. Check for potential original IPs.
- Verify no WAF is detected using:
  - **Wappalyzer extension**.
  - **wafw00f tool**.

## Method 10: Using VirusTotal
1. Use the following command in a terminal:
   ```bash
   curl -s "https://www.virustotal.com/vtapi/v2/domain/report?domain=<DOMAIN>&apikey=982680b1787fa59701919aa22515a025e00df1e3bb2bc4f186b8e919558d576c" | jq -r '.. | .ip_address? // empty' | grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}'
   ```
   Replace `<DOMAIN>` with the target website URL (without brackets).
2. Filter results using:
   ```bash
   curl -s "https://www.virustotal.com/vtapi/v2/domain/report?domain=<DOMAIN>&apikey=982680b1787fa59701919aa22515a025e00df1e3bb2bc4f186b8e919558d576c" | jq -r '.. | .ip_address? // empty' | grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}' | httpx-toolkit -sc -td -title -server
   ```
3. Check all active IPs to identify the original IP.
- Verify no WAF is detected using:
  - **Wappalyzer extension**.
  - **wafw00f tool**.

---

**Note**: This guide is intended for ethical hacking and penetration testing purposes. Always ensure you have the necessary permissions before performing any tests.

