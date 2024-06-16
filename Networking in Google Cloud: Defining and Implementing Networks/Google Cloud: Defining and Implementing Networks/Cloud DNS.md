### Cloud DNS
**DNS**:
- It translates domain name to IP addresses.
- Enabling browsers and other services to locate and interact with internet resources.
- Provided by your ISP.

**Working**:
1. A client makes a DNS request to obatain IP address the request is sent to a recursive resolver.
2. A recursive resolver requests the IP address from a name server.
3. The name server responds with the IP address.
4. The recursive resolver sends the IP to the client.
<br>**Note:** When the name server cannot satisfy the lookup, the DNS service might contact another DNS service for this information.

**Private and Public DNS zones**:

| **Feature**              | **Private DNS zones**                                                                                                        | **Public DNS Zones*                                                                                  |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| **Provides**             | Namespace that is visible only inside the VPC or hybrid network environment.                                                 | Authoritative DNS resolution to clients on the public internet.                                      |
| **Example**              | An org. would use a private zone for a domain dev.gcp.example.com which is reachable only from within the company intranet.  | Business would use a public zone for its external website i.e accessible directly from the internet. |

### Working of DNS in Google Cloud: A Primer with Real-Life Example

#### Scenario: Hosting `www.bakery.com` on Google Cloud
1. **Domain Registration**:
   - The bakery registers the domain `bakery.com` through a domain registrar.

2. **Creating a Managed Zone**:
   - The bakery creates a managed zone in Google Cloud DNS named `bakery-zone`.
   - Command:
     ```sh
     gcloud dns managed-zones create bakery-zone --dns-name="bakery.com." --description="Managed zone for bakery.com"
     ```

3. **Adding DNS Records**:
   - The bakery adds an A record for `www.bakery.com` pointing to the IP address of their web server.
   - Commands:
     ```sh
     gcloud dns record-sets transaction start --zone="bakery-zone"
     gcloud dns record-sets transaction add --zone="bakery-zone" \
         --name="www.bakery.com." --type="A" --ttl="300" "192.0.2.1"
     gcloud dns record-sets transaction execute --zone="bakery-zone"
     ```

4. **Updating Nameservers**:
   - The bakery updates their domain registrar to use Google Cloud DNS nameservers (e.g., `ns-cloud-a1.googledomains.com`).

5. **DNS Resolution Process**:
   - When a user types `www.bakery.com` into their browser, the browser performs a DNS query.
   - The query is sent to a DNS resolver, which checks its cache or queries Google Cloud DNS nameservers.
   - Google Cloud DNS resolves `www.bakery.com` to `192.0.2.1`.
   - The browser then connects to `192.0.2.1`, displaying the bakery's website.

### Benefits:

- **Scalability**: Google Cloud DNS automatically scales to handle large volumes of DNS queries.
- **Reliability**: Built on the same infrastructure as Google’s own services, ensuring high availability.
- **Security**: Supports DNSSEC to protect against DNS spoofing and other attacks.

### Summary

- **Google Cloud DNS**: Manages domain name resolution using a scalable and reliable service.
- **Managed Zones**: Container for DNS records of a domain.
- **DNS Records**: Entries like A, CNAME, MX, etc., that map domain names to IP addresses or other information.
- **Nameservers**: Google Cloud’s DNS servers that resolve domain names.
- **Real-Life Example**: Register domain, create managed zone, add DNS records, update nameservers, and access the website.

This setup ensures that `www.bakery.com` is reliably resolved to the correct IP address, allowing users to access the bakery's website seamlessly.

================================================================

### DNS Policies:
- Cloud DNS policies provides a flexible way to refine how your org. uses DNS.
- After you create the DNS records and artifacts needed for lookups, create Cloud DNS policies.

  **Supported Cloud DNS policies**:
         **Server Policies**:
                   - Apply private DNS config. to a VPC network.
                   - Set up the hybrid deployments for DNS resolutions.
                   - You can setupan inbound server policy depending on the direction of DNS resolutions.
         **Response Policies**:
                   - Enable you to modify the behaviour of the DNS resolver by using rules that you define.
                   - Is a Cloud DNS private zone concept that contains rules instead of records.
  Lets you introduce customized rules in DNS servers within your network that DNS resolver consults during lookups.
         **Routing Policies**: Steer traffic based on geolocation or round robin.
