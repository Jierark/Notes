In the early days of the Internet, it was very trivial for anyone to host and access websites. It was also during this time when web censorship started to occur, particularly with governments. Nowadays, censorship methods have become much more involved, with the Great Firewall of China(GFC) being the most notable example.
# Early Censorship Methods
The simplest methods of censorship were modifying or blocking DNS requests, or blacklisting specific IP addresses. HTTPS prevented some of these methods with encryption, but the rise of Deep Packet Inspection does negate some of these. DPI can infer data based on packet characteristics. There are many other methods of detection and evasion, such as the use of VPNs, ToR, etc.

# The Onion Router (ToR)
ToR works by routing traffic through multiple relays to evade censors. There are two types of relays:
- Public relays are listed in an accessible directory, often being the initial access point for most ToR users. However, these relays can be easily blocked due to their public nature.
- Private relays, or bridges, are unlisted relays that are shared through rate-limited channels, such as bittorent. These are specifically designed to evade detection for users in censored regions.
An interesting question: how many relays are needed for the best amount of security?
- 1 relay is obviously too insecure, as it would only take the compromising of that single relay. Notably, this is how VPNs essentially work.
- 2 relays is also not quite the right number. If one of the routers were compromised, then those packets could easily be linked to the next router.
- 3 relays is generally good enough, as this linking is much harder to prove.

Through it's history, ToR used different metrics to disguise any ToR traffic as random or benign flows.
- obfs2 added a simple obfuscation scheme, but it was passively detectable.
- obfs3 introduced a Diffie-Hellman key exchange to disguise traffic as random without the secret.
- obfs4/ScrambleSuit required the shared secret and was much more resistant to active probing.

# Active Probing
Previous methods were only able to infer what servers were doing based on the packets they could observe, which then stopped working with developing obfuscation schemes. Active Probing instead directly connects to servers to confirm the presence of some circumvention tool. Typically, these targeted servers are found by passively monitoring for "suspicious" TLS connections. This is one of the main methods that drives the GFC and was uncovered by researchers that ran controlled Tor infrastructures and monitored connections. It also gave some insight into how to design better circumvention techniques. This is still a continuing arms race, as probes find new ways to identify servers and servers find new ways to block probes while allowing clients through.

# Encore
Outside of the previously controlled experiment, measuring censorship turns out to be a difficult task. There are usually few ways to get access to a point inside a censored network or area, persistence is difficult, and asking for users is a pretty terrible idea as it puts them at risk.

The paper about Encore, controversially, used an interesting technique to try to measure censorship using cross-origin requests. Cooperating web pages would embed a small request for an image or script to a suspected blocked site, and client browsers would invisibly try to fetch them. A failure to retrieve that resource indicates some kind of censorship, and results were sent to a collection server. These are all possible to implement in Javascript, relying on specific browser events.

The results showed the effectiveness of this method, working better than active measurement tools since it required no interaction from users and ran automatically. However, the paper was met with a ton of ethical debate as participants (the users that visited sites) had no way to consent to this browser activity, which could get them in trouble in an repressive regime. But the inclusion of an opt in/out button could also skew results...

I personally think it would be better if they were able to commandeer a set of machines or devices within the place of interest to run experiments, but that's likely too difficult to do, especially for purposes of measuring censorship. I imagine governments wouldn't be too happy about that.