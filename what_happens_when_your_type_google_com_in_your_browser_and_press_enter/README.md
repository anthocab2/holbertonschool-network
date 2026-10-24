# What Happens When You Type a URL and Press Enter?

This project explains the infrastructure behind navigating to
`https://www.google.com`, from DNS resolution to the rendered page.

## Task 0: What happens when...

Read the [published article on Medium](https://medium.com/@anthonycaban1010/what-happens-when-you-type-https-www-google-com-in-your-browser-and-press-enter-080383ce98f8)
or its source in [blog_post.md](./blog_post.md).
It covers all eight required topics:

| Required topic | Article section |
| --- | --- |
| DNS request | 2. DNS finds an IP address |
| TCP/IP | 3. TCP/IP carries the conversation |
| Firewall | 4. Firewalls decide which traffic is allowed |
| HTTPS/SSL | 5. HTTPS uses TLS to secure the connection |
| Load-balancer | 6. The HTTP request reaches a load balancer |
| Web server | 7. The web server handles HTTP |
| Application server | 8. The application server runs the logic |
| Database | 9. Databases and indexes supply stored information |

The article also explains URL parsing, HTTP/3 over QUIC, caching, and browser
rendering. It distinguishes general architectural examples from documented
Google behavior. Technical sources are linked beside the relevant explanations.

## Publication and submission

**Status: published on Medium.**

[0-blog_post](./0-blog_post) contains the public article URL as a single line,
followed by a newline. The article and README are written in English.

Submit the same **Medium article URL** in the intranet field. GitHub stores the
answer file and article source; the submitted blog URL points to Medium.

## Files

| File | Purpose |
| --- | --- |
| `0-blog_post` | Public Medium article URL for task 0 |
| `blog_post.md` | Complete article source with technical references |
| `README.md` | Project overview, topic coverage, and submission instructions |

The content was checked against the eight required topics. The published page,
answer-file format, and documentation links were verified. The intranet review
has not been performed.

The directory spelling matches the assignment exactly:
`what_happens_when_your_type_google_com_in_your_browser_and_press_enter`.
