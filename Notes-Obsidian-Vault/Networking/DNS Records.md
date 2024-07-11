## Table of Contents

    - [Step 5: Configuring Your Ubuntu Server for DNS](#Step\5:\Configuring\Your\Ubuntu\Server\for\DNS)
    - [Step 6: Configuring Netcat Services](#Step\6:\Configuring\Netcat\Services)

1. **Domain Registration**: If you haven’t already, register a domain name with a domain registrar.
    
2. **Set DNS Records at Registrar**:
    
    - Point your domain (e.g., `ctfsite.com`) to your Linode's IP address using an A record.
    - If you use subdomains (like `pwn1.ctfsite.com`), set A records for each, pointing to the same Linode IP.
3. **Configure Reverse DNS in Linode**:
    
    - This step is optional but recommended for reverse lookups.
    - In the Linode dashboard, set the reverse DNS to match your domain.

### Step 5: Configuring Your Ubuntu Server for DNS

1. **Install DNS Tools** (Optional):
    
    bash
    

1. `sudo apt install dnsutils`
    
2. **Test DNS Configuration**:
    - Use `dig` or `nslookup` to ensure your domain points to your Linode IP.

### Step 6: Configuring Netcat Services

1. **Ensure Containers are Netcatable**:
    - Each Docker container running a service should be accessible via netcat using the Linode's IP and the mapped port.
    - Test with `nc <linode_ip> <mapped_port>`.