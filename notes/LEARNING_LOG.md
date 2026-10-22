# Learning Log

## 2026-10-20: URLs and DNS

Client vs server
- Client always asks (sends a request). Example: my browser, curl, git.
- Server waits for requests, processes them, and sends back a response. It never talks first.
  Example: GitHub's server sent my repo back when I ran git clone.
- The client is the program (git, curl, browser), not the Terminal window.
  The command (git clone) is the request.

5 parts of a URL
Example: https://www.youtube.com/watch?v=Va1qy7H1xdw&t=60
- Scheme: https (how we talk)
- Host: www.youtube.com (which machine)
- Port: 443 (not written, https uses 443 by default; http uses 80)
- Path: /watch (which page)
- Query: v=Va1qy7H1xdw&t=60 (settings as key=value, ? starts them, & joins them)

DNS
- DNS is like a phonebook: it looks up the IP address for a name. Then my client calls that IP.
  Example: dig github.com -> 140.82.114.4 (DNS itself uses port 53)
- A record = name -> IPv4 address. IPv4 looks like 140.82.114.4,
  IPv6 is the newer, longer format (2606:4700:10::6814:179a).
- TTL = how many seconds the answer can be cached before asking again (dig showed 41).
- "Non-authoritative answer" = my router answered from memory, not github's own DNS server.

Git basics (bonus)
- Git stores 3 kinds of objects: blob (file contents), tree (folder listing with
  file names), commit (snapshot + who + when + message + parent).
  Seen with: git cat-file -p HEAD -> tree -> blob.
- Working folder -> git add -> staging area -> git commit -> history -> git push -> GitHub.
- Git does not track empty folders. Re-run git add after editing a staged file.

What surprised me
- One name has many servers (google.com gave 6 IPs).
- A wrong ? instead of & broke my YouTube link, so syntax matters.
- If no port is written, the default is used automatically.

## 2026-10-22: TCP, ports, HTTPS

Order of every HTTPS request
DNS (name -> IP) -> TCP (connect to port 443) -> TLS (encrypt + verify) -> HTTP GET -> 200 OK

TCP
- TCP builds the connection between my computer and the server with a handshake
  (SYN, SYN-ACK, ACK). It also makes sure data arrives complete and in order.
  In curl -v: "Connected to example.com ... port 443".

Ports
- A server runs many programs; the port picks which one gets my request.
  HTTPS = 443, HTTP = 80, DNS = 53, SSH = 22.
- Full address = IP + port, like 140.82.114.4:443 (building + apartment number).

TLS (the "S" in HTTPS)
- Adds two things: encryption (nobody in between can read the data) and
  verification (the server shows a certificate signed by a trusted CA; my Mac
  checks it against its own list of trusted CAs in /etc/ssl/cert.pem).
- example.com certificate: subject example.com, issuer Cloudflare CA,
  expires Dec 25 2026, "SSL certificate verify ok".
- Without a certificate, an attacker could pretend to be the site
  (man-in-the-middle) and steal passwords.

Reading curl -v
- * = curl's notes, > = what I sent, < = what the server sent back.
- HTTP/2 200 = success. cf-ray ...-ATL = served from Cloudflare's Atlanta location (cache HIT).

Expired certificate (expired.badssl.com)
- DNS and TCP worked, but my Mac refused the certificate ("certificate has expired")
  and closed the connection. No GET request was sent. curl exit code 60.
- My computer (the client) refused, not the server. Like an expired passport at a counter.

http vs https
- http://example.com used port 80, had no TLS lines, and sent GET as plain text
  (anyone on the Wi-Fi could read it). It used HTTP/1.1.
- https used port 443, TLS 1.3, a verified certificate, and HTTP/2.
- Never type a password on an http:// site.
