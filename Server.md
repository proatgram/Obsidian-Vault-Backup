# Apps I'm planning on tinkering with or running
---
All of these are subject to change, and *will probably* change due to me just experimenting with different things, trying them out, etc. This list is more or less what I thought of off the top of my head, either based on things I have a slight interest in hosting (either because it seems cool and interesting, or because it would possibly be useful) or have hosted already in a small scale.

 - GitLab (and their respective CI/CD runners)
 - PasteBin software
 - Drag and Drop file share
 - EMail
 - Quite possibly a personal blog, maybe
 - Custom Matrix instance
 - Game servers (Interested in Modded Minecraft, but some mods, like IG Voice Chat require more ports than what playit.gg can offer which is what I previously used for this)
 - Password Manager
 - Possibly some Software Development tools
 - URL Shortener


# Reasons to have a domain
---
## New hobby
I would like something to do, I have seen some videos and some posts of peoples' setups which looked really awesome, I have been in need of a new hobby and this has peaked my interest. I began tinkering around with server stuff a while before I asked about the domain, but I was pretty limited on what I could do. A lot of the "basic" applications I hosted successfully were kinda useless without a way to share a custom URL. For example a PasteBin software, used to share say error logs, coredumps, configurations, etc, would need a way to share whatever text you save on it, hence a need for a public domain that you can easily pass around. 

## Broken / semi-unfunctional apps
Another reason, is some apps are broken without it. To give some examples, I had a hard time getting GitLab to output a reasonable redirect URL for me to sign on and / or create an account, manage CI/CD runners, etc, as it would spit out a garbled URL. Another example, which doesn't even work to it's full potential without the domain, is some software called Forte, a self-hosted music platform which is decentralized and federated. Now, self hosting software to stream music, either from other federated servers or locally, would be almost completely useless without a way to actually access them when you actually want to, for example on a trip, while traveling, at school even when allowed to.

## Organization
Having a domain will allow me to organize the applications that I run and host on it in a pleasing way. For example, if I were to run GitLab, instead of having to specify the address `localhost:6789` or maybe even `170.10.0.4:6789` if I were to go through a VPN, then having a domain (then paired with a subdomain for the specific app) like `git.thetimbrick.dev` would make it much easier to remember. This will allow me to differentiate between different apps that I have running.

## Accessibility
Having a domain will also allow me to have my work, whether it be through my GitLab, a blog, portfolio, or even just bare Git server, be able to be discovered by people. If I had my portfolio on my server, but no one could access it, that wouldn't be that useful. Same for my GitLab, I use my projects, my README / description on my profiles, Git history, etc to show my work, how good I am, what I can do, to people who might want to work with me, maybe a major company, or just a team of 10 wanting to get something done. In addition to that, having a Git server running on a server that is not accessible to places where I might need it would be a problem. I might need to have access to that in order to push a critical fix to a bug.

# Security
---
With a public server comes a need for security. Using tools that I have and will continue to learn how to use effectively I will secure the server.

See [Self-Hosting-Guide#security](https://github.com/mikeroyal/Self-Hosting-Guide#security) for more applications that could be run.

## Firewalld
Firewalld is a firewall application that is highly advanced and allows for configurations based on a multitude of different situations. Using this I can block many different ports to fight against different attacks that might happen.

## Fail2Ban
This software will scan logs and detect malicious activity performed by IP addresses and ban them.

## Cloudflair Tunnel
Exposing a port on our home network poses a major security risk as people could target our home, find our public IP, DDOS, and even more. This isn't important in this context, but our IP is not static, making it hard for a domain name to be kept with the IP. Using [Cloudflair tunnels](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/), a user of the domain could connect to the domain, which would have a flow similar to this:

External browser HTTP Request -> Cloudlfair Network / Server HTTP Request -> Cloudflaird Software running on server -> Local service.

This has the effect of hiding our public IP behind Cloudlflair and having their IP behind it.

![](https://developers.cloudflare.com/assets/handshake-dc58ec8d.jpg)