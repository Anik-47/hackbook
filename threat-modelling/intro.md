# Intro

## Key Terms

**Vulnerability:**\
It is a flaw, loophole, or error that can be exploited to violate security policies. For example, you found an application that is susceptible to buffer overflow exploit.\
\
**Threat:**\
A threat is an event that can be natural or man-made which can impact an organisation negatively. It could be a natural calamity like an earthquake or a hacker.\
\
**Exploit:**\
An exploit is a defined way to breach the security of an IT system through a vulnerability. For example, a piece of code that can be used to execute a certain attack  based on the vulnerability\
\
**Risk:**\
It is the probability of an event or situation involving exposure to danger, for example, the likelihood of a vulnerability being exploited.

## Threat Modelling

It's a security process where potential threats are identified, categorized, and analyzed.

There are two approaches to implementing threat modeling&#x20;

1\) **Proactive Approach:**  Taking necessary control during design and development.

2\) **Reactive Approach**:   Implementing control once the product has been deployed.&#x20;

### Goals of Threat Modeling:

1. To reduce the number of security-related  gaps in coding or architecture
2. To reduce the severity of the remaining vulnerability

### Identifying Threats

1. Focused on Attackers: Identifying potential attackers and their goals.
2. Focus on Assets: Identifying threats to valuable assets.
3. Focus on Software: Potential threats against developed software.

## Threat Modeling Standard

### STRIDE

```

Spoofing: In cyber-security spoofing is an attack when an attacker pretends to be 
someone else.

Tampering: Unauthorised modification, editing, or manipulation of data.

Repudiation: Sending proof of identity to the recipient and the receiver should have
proof of identity of the sender. (Digital Certificate)

Information Disclosure: Disclosure of sensitive information

DOS: Sending an unauthorized large volume of packets, it can be TCP or UDP

Elevation of Privilege: Unauthorized access to a higher level of user, being 
a normal user.

```

### Dread

```
It decides the impact of a threat

Damage Potential: To measure the severity of the damage
Reproducibility: How easy it is to reproduce the exploit 
Exploitability: How easy it is to exploit
Affected User: Number of Users affected by the exploit
Discoverability: How easy it is to find the vulnerability
```

