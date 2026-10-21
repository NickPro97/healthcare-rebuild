## 2026-10-20: URLs and DNS

Client vs server
- Client always asks (sends a request). Example: my browser, curl, git.
- Server waits for requests, processes them, and sends back a response. It never talks first.
  Example: GitHub's server sent my repo back when I ran git clone.

5 parts of a URL
Example: https://www.youtube.com/watch?v=Va1qy7H1xdw&t=60
- Scheme: https (how we talk)
- Host: [www.youtube.com](https://www.youtube.com) (which machine)
- Port: 443 (not written, https uses 443 by default; http uses 80)
- Path: /watch (which page)
- Query: v=Va1qy7H1xdw&t=60 (settings as key=value, ? starts them, & joins them)

DNS
- DNS is like a phonebook: it looks up the IP address for a name. Then my client calls that IP.
  Example: dig github.com -> 140.82.114.4 (DNS itself uses port 53)

What surprised me
- One name has many servers (google.com gave 6 IPs).
- A wrong ? instead of & broke my YouTube link, so syntax matters.
- If no port is written, the default is used automatically.