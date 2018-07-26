---
layout: post
title: DDoS through botnet and Proactive Counter Measures
tag: penetration
---

> This is a academic article!

## Introduction

With the rising of Internet service, the practice on attacking the server in order to block- ing users from getting service emerged, which is Denial of Service attack, dated back to 1990s[1]. As the server becoming more powerful, new attacks are derived utilizing compromised machines on a network as attacking power[2].

This survey report would focus on understanding dos attack through the botnet, which is widely spread due to its easiness by using toolkits like blackenergy [3], and a lot of countermeasures are developed such as firewalls and authentications.

Denial of service attack is crucial to the modern Internet due to its ability of significantly reducing service quality providing to legitimate users, and even worse, bring down a server and damaging data permanently if effective countermeasure is not properly deployed.

## Denial of Service attack through botnet

DoS attack used to be flooding server from several machines owned by attacker, modern powerful machines cannot be easily overwhelmed, so attackers now try to overtake a bunch of devices to perform attack on higher layers with heavier loads[4], this technique is normally considered as botnet based distributed denial of service attack[5].

### Botnet technology
Botnet is a bunch of systems that are compromised on a network, they are connected and controlled by a remote host, called “botmaster”[6]. With the rapidly increased number of network devices, attackers now can easily find enough systems to control.

Attackers can perform botnet deployment by using malwares, for example, Blackenergy, which causes the service shutdown in Ukrainian energy grid, and in this case, virus is transfered into the victim machines as a Microsoft office file[3]. After deployment, it could remain undetected and then later be remotely instructed to perform scripted tasks. 

Botnets can perform different tasks, some may collect credentials, and in denial of service attack case, it would flood the server with data to process.

### Botnet HTTP-GET ddos attack

DoS attacks are normally undertaken in the form of packet flooding in the network layer, the newly emerged HTTP-GET attack is more complicated and undetectable against the application layer[2], an example would be the MyDoom[7].

When a legitimate user access a web server from a company, assume the user would like to download a 50Mb file, using HTTP, through GET request. Then the web server would need to retrieve data from database and then send out to the user, which would take processing power; now consider an attacker control the botnet consisting of 100 thousand systems simultaneously doing the same HTTP-GET request, they would all be treated as valid operation by the web server because they are doing legitimate request, the attacker could command them to perform this task multiple times, then the web server is very likely to be congested and cannot provide service to legitimate users.

This HTTP-GET attack can bypass most countermeasures due to the fact it is exploited application level protocols, and by the layered model design, each layer would have limited access to its upper layer[8]. While most firewalls are targeting at IP address and port numbers[9], they would not read the HTTP or HTTPS packets for verification, and even if they do, the request is legit.

## Effective countermeasures

In order to detect and prevent this attack, two proactive and offensive countermeasures are taken considered, which are killbot, it would identify the bots from the real human, and denied its access[10], and honeynet, which traps the attacker[11].

### Killbots

This countermeasure takes the aspect to identify the botnet from legitimate users without looking into the actual packets by utilizing the human authentication and admission control[10].

In authentication, each new access would be requested to do a reverse Turing test on
human verification, then later, all access considered zombies would be denied. However, this measure would need to balance between processing new verification and maintaining verified access, which would waste resource[10].

Admission control is then introduced, it would calculate a probability that an access need to be authenticated. Then when a new one income, it would check whether it is a zombie, if not, a probability would be generated on whether it should be verified, if so, then the one passed could get a temporary pass. Finally, those with pass could setup connection[10].

### Honeypot and honeynet

Honeypot is a kind of firewall, that act like a legitimate server of attacker’s target, it would let the attacker penetrate in and make it believe the attack is succeeded. Then the attacking information would be analyzed and thereby the actual server could be prepared to defend the attack[12].

Honeynet uses the same logic but with a larger scale, it lures the attacker and analyzes its attack pattern. This countermeasure could be done in a larger and more diverse network environment and could be used to mimic multiple server cascading, it fits the modern network environment and models more properly[11].

Moreover, honeynet could also capture the bot, by reverse engineering and backtrack its IP and even find the attacker, then letting the security engineers overtaken the botnet[13][14].

## Countermeasures insufficiency

Killbots and honeynet are an advanced defense mechanism against botnet based DoS attack. Application of these two methods could reduce the risk of servers being inaccessible to legitimate users. However, neither one can solely handle the attack, hence there exists insufficiency.

Killbots introducing probability on access verification, which would lead to a legitimate user being dropped from connection without given verification due to its probability does not admit it too. This is unacceptable for most business since the user would receive unreasonable connection abortion by being treated as zombies. Also, if the reverse Turing test is compromised, the servers would be left undefended.

Honeynet and the honeypots are essentially just another servers. Under the circumstances that attackers’ botnets have the huge computing power, it could also break down the honeynet. Another shortcoming is that this countermeasure is not economically efficient for small and medium business, since they normally only have one or two servers, hosting an extra one for attracting attack is expensive.

And both countermeasures do not tackle the problem that a botnet access HTTP packet is the same as legitimate user’s ones. When the botnet becoming active immediately after those compromised machines successfully access the target server, and then undertaking repeat attack to flood it, in this case, the attack bypass the firewall using a human first access, and leaving authentication and honeynet useless.

## Conclusion

Botnet-based DoS attack using HTTP-GET method is very hard to counter. In theory, it could not be detected by firewalls due to the fact it is the same data transmitted by legitimate users. Even though both killbot and honeynet could prevent the attack, identify bot from human, and even backtrack “botmaster”, basic flaws exists which is they cannot distinguish bot and human by simply observing HTTP request packets, hence as long as the attacker could find a way to bypass authentication mechanism, the attack could not be prevented by these two methods. However, these countermeasures provide certain level protection and make the attack be difficult to deploy.

## Future study

Based on the discussion and conclusion, the future study aspect is clear, that is to distinguish a bot sent packets from human sent packets. One of the possible direction is to study the network behavior using machine learning[15], however, simply analyzing the network behavior during an attack may not be enough, and including user's normal network behavior may violate privacy. Hence this remains a future study topic.

# Bibliography

1. R. M. Needham, “Denial of service,” in Proceedings of the 1st ACM Conference on Computer and Communications Security, pp. 151–153, ACM, 1993.
2. E. Alomari, S. Manickam, B. Gupta, S. Karuppayah, and R. Alfaris, “Botnet-based distributed denial of service (ddos) attacks on web servers: classification and art,” arXiv preprint arXiv:1208.0403, 2012.
3. D. U. Case, “Analysis of the cyber attack on the ukrainian power grid,” Electricity Information Sharing and Analysis Center (E-ISAC), 2016.
4. Y. Xie and S.-Z. Yu, “Monitoring the application-layer ddos attacks for popular websites,” IEEE/ACM Transactions on Networking (TON), vol. 17, no. 1, pp. 15– 25, 2009.
5. M. Feily, A. Shahrestani, and S. Ramadass, “A survey of botnet and botnet detec- tion,” in Emerging Security Information, Systems and Technologies, 2009. SECUR- WARE’09. Third International Conference on, pp. 268–273, IEEE, 2009.
6. M. Eslahi, R. Salleh, and N. Anuar, “Bots and botnets: An overview of characteris- tics, detection and challenges,” in Proceedings - 2012 IEEE International Conference on Control System, Computing and Engineering, ICCSCE 2012, pp. 349–354, 11 2012.
7. H. Chau, “Network security–mydoom, doomjuice, win32/doomjuice worms and dos/ddos attacks.”
8. A. S. Tanenbaum, “Computer networks, /andrew s. tanenbaum, david j. wetherall,” Cloth: Prentice Hall, 2011.
9. W. R. Cheswick, S. M. Bellovin, and A. D. Rubin, Firewalls and Internet security:
repelling the wily hacker. Addison-Wesley Longman Publishing Co., Inc., 2003.
10. S. Kandula, D. Katabi, M. Jacob, and A. Berger, “Botz-4-sale: Surviving organized ddos attacks that mimic flash crowds,” in Proceedings of the 2nd conference on Sym- posium on Networked Systems Design & Implementation-Volume 2, pp. 287–300, USENIX Association, 2005.
11. L. Spitzner, “The honeynet project: Trapping the hackers,” IEEE Security & Pri- vacy, vol. 99, no. 2, pp. 15–23, 2003.
12. N. Weiler, “Honeypots for distributed denial-of-service attacks,” in Enabling Tech- nologies: Infrastructure for Collaborative Enterprises, 2002. WET ICE 2002. Pro- ceedings. Eleventh IEEE International Workshops on, pp. 109–114, IEEE, 2002.
13. M. Sacchetin, A. Gregio, L. Duarte, and A. Montes, “Botnet detection and analysis using honeynet,”
14. F. C. Freiling, T. Holz, and G. Wicherski, “Botnet tracking: Exploring a root-cause methodology to prevent distributed denial-of-service attacks,” in European Sympo- sium on Research in Computer Security, pp. 319–335, Springer, 2005.
15. S. Saad, I. Traore, A. Ghorbani, B. Sayed, D. Zhao, W. Lu, J. Felix, and P. Hakimian, “Detecting p2p botnets through network behavior analysis and machine learning,” in Privacy, Security and Trust (PST), 2011 Ninth Annual International Conference on, pp. 174–180, IEEE, 2011.