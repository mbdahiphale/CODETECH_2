# CODETECH_2

Name : Mahesh Bajirao Dahiphale 
Company : CODTECH 
ID : CT08DS4465 
Domain : Cyber Security and Ethical Hacking 
Duration : july to august 2024

This Python script provides functionality to scan common ports on a target IP address and check the reachability of a given URL. Here's an overview of its components and functionality:

### Components and Functionality

1. **List of Common Ports**:
   - `common_ports` is a predefined list of ports typically used for various services (e.g., HTTP, FTP, SSH).

2. **Port Scanning Function**:
   - `scan_ports(target)`: 
     - Iterates over the `common_ports` list.
     - Uses `socket` to attempt a connection to each port on the target IP address.
     - Collects and returns a list of ports that are open (`result == 0` indicates success).

3. **URL Reachability Check Function**:
   - `check_url_reachability(url)`:
     - Attempts to establish a TCP connection to port 80 of the given URL (HTTP).
     - Sends a `HEAD` request and checks the response to determine if the URL is reachable.
     - Handles exceptions to indicate if the URL is not reachable.

4. **Main Function**:
   - Prompts the user to enter the target IP address and URL.
   - Calls `scan_ports` to identify open ports on the target IP.
   - Calls `check_url_reachability` to check if the specified URL is reachable.
   - Prints the results of both the port scan and URL reachability check.

### Example Usage

When you run the script, it will:

- Prompt you to enter the target IP address.
- Scan the specified ports and display the open ones.
- Prompt you to enter a URL.
- Check the reachability of the URL and print whether it is reachable or not.

### Code Improvements

- **Error Handling**: Enhance error handling, especially in network connections.
- **Timeout Configuration**: Fine-tune socket timeouts based on the expected network conditions.
- **Logging**: Implement logging for better traceability of scan results and errors.

### Complete Code for Reference


import socket

# List of common ports to scan
common_ports = [21, 22, 23, 25, 53, 80, 110, 135, 139, 143, 443, 445, 993, 995, 1723, 3306, 3389, 5900, 8080]

def scan_ports(target):
    print(f"Scanning ports on {target}...")
    open_ports = []
    for port in common_ports:
        try:
            sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            socket.setdefaulttimeout(1)
            result = sock.connect_ex((target, port))
            if result == 0:
                open_ports.append(port)
            sock.close()
        except Exception as e:
            print(f"Error scanning port {port}: {e}")
    return open_ports

def check_url_reachability(url):
    print(f"Checking reachability of {url}...")
    try:
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        sock.settimeout(5)
        sock.connect((url, 80))
        sock.send(b"HEAD / HTTP/1.1\r\nHost: " + url.encode() + b"\r\n\r\n")
        response = sock.recv(1024)
        sock.close()
        if b"200 OK" in response:
            return "URL is reachable"
        else:
            return "URL is not reachable"
    except Exception as e:
        return f"URL is not reachable: {e}"

def main():
    target = input("Enter the target IP address: ").strip()
    
    # Network and Port Scanning
    open_ports = scan_ports(target)
    print(f"Open ports on {target}: {open_ports}")
    
    url = input("Enter the URL to check (without http:// or https://): ").strip()
    
    # URL Reachability Check
    reachability = check_url_reachability(url)
    print(reachability)

if __name__ == "__main__":
    main()
```

This script is a basic tool for network diagnostics and should be used responsibly, respecting all applicable laws and regulations.
