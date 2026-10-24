# Cybersecurity Study Notes: Network Traffic, APTs, IoT, Firewalls, Endpoint Security, and Software Updates

## Module 1: Data Transmission Interception Tools

### 1.1 Understanding data interception

**Data transmission interception** involves capturing and examining data as it travels across a network. These techniques have legitimate defensive uses, such as troubleshooting and security monitoring, but can also be abused to steal information.

A **packet** is a unit of data transmitted over a network. A packet analyzer captures and interprets packets to help users understand network communications.

#### Man-in-the-middle (MitM) attacks

A **man-in-the-middle attack** occurs when an attacker positions themselves between two communicating parties to observe or potentially manipulate their communications.

Typical sequence:

1. A client attempts to communicate with a server.
2. An attacker intercepts or redirects the communication.
3. The attacker may observe, relay, or modify traffic.
4. The client and server may continue communicating without recognizing the interception.

Encryption, certificate validation, secure network configuration, and endpoint protection help reduce this risk.

### 1.2 Important traffic interception and analysis tools

| Tool | Primary function | Typical legitimate use |
|---|---|---|
| Wireshark | Graphical network protocol analysis | Troubleshooting and packet inspection |
| Cain & Abel | Legacy Windows password recovery and network analysis tool | Historical security analysis and authorized recovery |
| Ettercap | LAN traffic interception and MitM testing | Authorized security assessments |
| tcpdump | Command-line packet capture and analysis | Troubleshooting and incident investigation |
| TCPflow | Reconstructs and stores TCP connection data | Protocol analysis and debugging |
| Snort | Network intrusion detection and prevention | Detecting suspicious traffic |
| Suricata | Network threat detection and security monitoring | IDS/IPS and network security monitoring |
| Dsniff | Collection of network auditing and sniffing tools | Authorized penetration testing |
| TShark | Terminal-based Wireshark functionality | Automated packet capture and analysis |

**Important distinction:** Wireshark and tcpdump primarily help capture and inspect traffic. Snort and Suricata focus on detecting and potentially preventing suspicious network activity. Ettercap is particularly associated with MitM techniques.

Cain & Abel is a legacy tool; its inclusion in the course does not imply that it is suitable for modern production environments.

### 1.3 Wireshark

Wireshark is an open-source, graphical network protocol analyzer available for Windows, macOS, and Linux.

Its key functions include:

- Capturing packets from supported network interfaces.
- Displaying protocol information in a structured format.
- Filtering traffic to focus on relevant packets.
- Investigating connectivity failures and suspicious communications.
- Examining saved packet-capture files.

**Example:** If a computer cannot connect to a web server, a packet capture may reveal repeated connection attempts, TCP resets, DNS failures, or retransmissions.

Wireshark does not automatically reveal the contents of properly encrypted HTTPS traffic. It may expose metadata, such as IP addresses, packet sizes, timing, and connection patterns.

### 1.4 Cain & Abel

Cain & Abel was a Windows password recovery and network analysis tool. The course describes its capabilities as including:

- Network traffic sniffing.
- Password recovery and cracking.
- Analysis of password representations.
- VoIP conversation recording under vulnerable conditions.

The tool illustrates why sending credentials without encryption is dangerous.

Modern applications should use TLS-protected connections, secure password storage, and multi-factor authentication. Password hashes should not be described as simply “scrambled passwords”: secure password hashing is deliberately designed to resist recovery, particularly when used with unique salts and appropriate password-hashing algorithms.

### 1.5 Ettercap

Ettercap is associated with MitM attacks and security testing on local area networks (LANs).

Depending on the network and protocol, interception techniques may allow an attacker to:

- Observe communications.
- Attempt to capture exposed credentials.
- Modify traffic.
- Impersonate another network participant.

MitM testing must be limited to systems and networks for which explicit authorization exists.

### 1.6 tcpdump

tcpdump is a command-line packet analyzer used to capture and inspect network traffic.

Example commands for an authorized lab:

```bash
# List interfaces supported by the installed tcpdump version
tcpdump -D

# Capture a limited number of packets on an authorized interface
sudo tcpdump -i eth0 -c 20

# Filter traffic for a particular host
sudo tcpdump -i eth0 host 192.0.2.10 -c 20

# Read an existing packet capture
tcpdump -r sample.pcap
```

Replace `eth0` and the example address with the appropriate lab values. Interface names vary by operating system.

### 1.7 Additional tools

- **TCPflow:** Collects TCP stream data for later analysis.
- **Snort:** Uses detection rules to identify suspicious network activity and can support prevention.
- **Suricata:** Provides network intrusion detection, intrusion prevention, and security monitoring.
- **Dsniff:** Contains tools associated with network auditing and credential interception.
- **TShark:** Provides command-line packet analysis and supports scripted workflows.

### 1.8 Key takeaway

Network traffic analysis helps defenders understand communications, troubleshoot faults, and investigate security incidents. The same visibility can be abused, so capture only traffic you are authorized to inspect and protect any captured data.

---

## Module 2: Advanced Persistent Threats (APTs) in Action

### 2.1 What is an APT?

An **Advanced Persistent Threat (APT)** is a sustained, targeted intrusion in which an attacker seeks to maintain access to a victim's systems, often for espionage, intellectual-property theft, disruption, or strategic objectives.

The term describes the nature of a threat or campaign rather than one specific malware family.

- **Advanced:** The attacker may use sophisticated techniques and carefully selected tools.
- **Persistent:** The attacker attempts to maintain or regain access over time.
- **Threat:** The activity presents a meaningful risk to an organization.

### 2.2 Warning signs of an APT

| Indicator | What it may look like | Defensive response |
|---|---|---|
| Unusual network traffic | Unexpected outbound transfers or connections at unusual times | Review network flow records, DNS logs, and packet captures |
| Privilege escalation | Unexpected administrator privileges or permission changes | Audit account and access-control changes |
| Unexpected reboots or shutdowns | Systems restart without an approved maintenance reason | Correlate operating-system and security logs |
| Unknown software or files | Unrecognized processes, services, or scheduled tasks | Investigate file provenance and endpoint telemetry |
| Abnormal user behavior | Unusual login locations, times, or resource access | Review authentication logs and validate activity |
| Security controls disabled | Antivirus, firewall, or monitoring settings change unexpectedly | Investigate immediately and restore protection safely |
| Repeated malware infections | Malware returns after apparent cleanup | Investigate persistence mechanisms and the original entry point |

These indicators are not proof of an APT by themselves. Legitimate administration, software updates, and unusual but harmless user activity can produce similar signals. Investigations should correlate multiple sources of evidence.

### 2.3 Typical APT lifecycle

A simplified intrusion lifecycle may include:

1. **Reconnaissance:** Gathering information about the target.
2. **Initial access:** Entering through a vulnerability, stolen credentials, phishing, or another route.
3. **Execution and establishment:** Running malicious code or establishing a foothold.
4. **Persistence:** Maintaining access across sessions or restarts.
5. **Privilege escalation:** Obtaining higher permissions.
6. **Lateral movement:** Moving from one compromised system to another.
7. **Command and control (C2):** Communicating with attacker-controlled infrastructure.
8. **Collection and exfiltration:** Gathering and transferring information.
9. **Continued operations:** Maintaining access or conducting further activity.

Not every campaign follows this exact order. Some stages may overlap, be skipped, or recur.

### 2.4 Notable APT incidents and campaigns

| Incident or group | Course-highlighted event | Main lesson |
|---|---|---|
| Stuxnet | Discovered in 2010; targeted Iran's nuclear program | Cyber operations can affect physical and industrial processes |
| Operation Aurora | 2009 campaign targeting organizations including Google | Intellectual property and source-code repositories require protection |
| APT28 / Fancy Bear | 2016 DNC compromise and email leak | Stolen information can have major political consequences |
| Equation Group | Associated with sophisticated espionage capabilities | Advanced tooling can support long-term intelligence collection |
| Lazarus Group | Associated with the 2014 Sony Pictures attack | Cyberattacks can cause financial and operational damage |
| APT29 / Cozy Bear | Associated with espionage campaigns, including U.S. government targets | Government and diplomatic organizations are potential targets |
| Darkhotel | Targeted executives through hotel-related attack opportunities | Travel and temporary networks can introduce security risks |
| Moonlight Maze | Late-1990s espionage targeting U.S. government and military networks | Long-term cyber espionage predates modern APT terminology |

Attribution to a particular group or state can be uncertain. Reports should distinguish confirmed technical evidence from assessments of who was responsible.

### 2.5 APT detection and response

When suspicious activity is detected:

1. Preserve relevant logs and evidence.
2. Validate whether the activity is authorized.
3. Isolate affected systems when necessary and safe to do so.
4. Investigate affected accounts, endpoints, network traffic, and cloud resources.
5. Identify the entry point and persistence mechanisms.
6. Contain the threat, remove unauthorized access, and recover from trusted sources.
7. Rotate compromised credentials and revoke exposed sessions or tokens.
8. Monitor for recurrence and document lessons learned.

**Key takeaway:** APT defense requires continuous monitoring, layered security, careful incident response, and investigation of root causes—not merely deleting suspicious files.

---

## Module 3: Internet of Things (IoT) Threats

### 3.1 What is IoT?

The **Internet of Things (IoT)** is a collection of physical devices that use network connectivity to collect, exchange, or act on data.

Examples include:

- Smart speakers and lighting.
- IP cameras and digital video recorders.
- Smart locks and thermostats.
- Industrial sensors and automation controllers.
- Connected health-monitoring equipment.

IoT devices can improve convenience and automation, but insecure devices may expose private information or become entry points into larger networks.

### 3.2 Common IoT vulnerabilities

| Vulnerability | Explanation | Potential consequence |
|---|---|---|
| Weak authentication | Default, weak, shared, or missing credentials | Unauthorized access |
| Outdated firmware | Known vulnerabilities remain unpatched | Device compromise |
| IoT botnets | Compromised devices are controlled collectively | DDoS attacks and other malicious activity |
| Insufficient privacy protections | Excessive or poorly protected data collection | Exposure of personal information |
| Lack of standardization | Devices have inconsistent security features and protocols | Difficulties applying uniform controls |
| Insecure network services | Unnecessary or weakly protected services are exposed | Unauthorized access or data interception |
| Insecure ecosystem interfaces | Vulnerable APIs, mobile apps, or cloud interfaces | Compromise through connected services |
| Poor physical security | Devices can be accessed or tampered with physically | Configuration changes or unauthorized firmware access |

### 3.3 The Mirai botnet

Mirai became prominent in 2016. It infected vulnerable IoT devices, including routers and IP cameras, and assembled them into a remotely controlled botnet.

One major event was the October 2016 DDoS attack against Dyn, a DNS service provider. The disruption affected access to numerous prominent internet services.

The course highlights several important factors:

- Many devices retained default usernames and passwords.
- Compromised devices could be controlled as a group.
- A large collection of relatively small devices could produce substantial traffic.
- Public release of Mirai's source code contributed to the development of related variants.

A **distributed denial-of-service (DDoS) attack** attempts to overwhelm a target service with traffic or requests from multiple systems, making the service unavailable or unreliable.

### 3.4 IoT security best practices

1. **Change default credentials.** Use unique, strong passwords.
2. **Enable strong authentication.** Use MFA where supported, especially for cloud accounts and administrative access.
3. **Update firmware.** Apply trusted security updates promptly.
4. **Encrypt communications.** Prefer secure protocols and properly validated TLS connections.
5. **Segment the network.** Place IoT devices on a separate network or VLAN where practical.
6. **Monitor behavior.** Investigate unusual outbound traffic, unexpected connections, and unexplained configuration changes.
7. **Choose reputable manufacturers.** Check update policies and product support lifetimes.
8. **Disable unnecessary services.** Turn off unused remote administration, discovery, and other exposed features.
9. **Protect physical access.** Secure devices, reset mechanisms, and exposed network ports.
10. **Review privacy settings.** Minimize data collection and limit unnecessary permissions.

### 3.5 Example: Securing a home IP camera

A reasonable security plan would be to change the factory credentials, update firmware, enable MFA on its cloud account if available, disable unnecessary remote-access features, and place the camera on a separate network from personal computers.

If the camera unexpectedly begins sending large amounts of outbound traffic, investigate the device and its network connections rather than assuming that it is functioning normally.

**Key takeaway:** IoT security depends on device configuration, vendor support, network isolation, authentication, privacy controls, and monitoring.

---

## Module 4: Firewall Configuration

### 4.1 What is a firewall?

A **firewall** enforces traffic-control rules between networks or on an individual device. It decides which communications to permit or block based on configured policy and available traffic information.

Firewalls can help prevent unauthorized access, but they do not replace secure applications, endpoint protection, patching, or user awareness.

### 4.2 Main firewall types

| Type | Main function | Important characteristic |
|---|---|---|
| Packet-filtering firewall | Examines packet headers such as IP addresses and ports | Basic traffic filtering |
| Circuit-level gateway | Evaluates connection or session behavior | Focuses on connection establishment rather than full application content |
| Stateful inspection firewall | Tracks connection state | Evaluates packets in the context of active connections |
| Proxy / application-level firewall | Intermediates communications and can inspect application-layer traffic | Provides application-aware control |
| Next-generation firewall (NGFW) | Combines firewalling with features such as application awareness and intrusion prevention | Offers broader inspection and control capabilities |

The most appropriate type depends on the environment, traffic requirements, budget, and security policy. An NGFW is not automatically the best choice for every individual device or small network.

### 4.3 Firewall rules and policies

A firewall rule commonly specifies:

- **Source:** Where the traffic originates.
- **Destination:** Where the traffic is going.
- **Protocol:** For example, TCP, UDP, or ICMP.
- **Port:** The relevant service port, where applicable.
- **Direction:** Inbound or outbound.
- **Action:** Allow, block, or another supported action.
- **Scope and profile:** Which interfaces, networks, or firewall profiles apply.

A sound baseline is **default deny for unsolicited inbound traffic**, with narrowly scoped exceptions for required services. Outbound rules should also reflect organizational risk and operational needs.

Rules should be ordered and reviewed carefully because an overly broad rule can undermine more restrictive controls.

### 4.4 Windows Defender Firewall configuration

The course describes this general procedure:

1. Open Control Panel.
2. Navigate to System and Security.
3. Open Windows Defender Firewall.
4. Select Advanced settings.
5. Choose Inbound Rules or Outbound Rules.
6. Select New Rule.
7. Choose the rule type, such as program or port.
8. Specify the conditions and choose whether to allow or block the connection.
9. Name the rule and finish.

**Practical caution:** Do not create a broad allow rule simply to make an application work. Verify the required executable, port, direction, network profile, and remote-address scope first. Avoid disabling the firewall as a troubleshooting shortcut.

### 4.5 macOS firewall configuration

The course's macOS Ventura instructions describe enabling the firewall in System Settings and using its options to control incoming connections for applications.

Interface labels vary across macOS versions. On modern versions, look under **System Settings → Network → Firewall**, or use the operating system's search to locate Firewall settings.

Stealth mode can reduce responses to some unsolicited probes, but it does not make a Mac invisible or replace other security controls.

### 4.6 Network Address Translation (NAT)

**Network Address Translation (NAT)** changes IP address information as traffic passes through a network device.

A common home-network example is source NAT/PAT, which allows multiple private-address devices to share a public IPv4 address.

NAT can reduce direct exposure of internal addresses, but **NAT is not a firewall and is not sufficient by itself as a security control**. Explicit firewall policy is still required.

### 4.7 Firewall maintenance

- Apply security and firmware updates.
- Remove obsolete or duplicated rules.
- Review permitted services and remote access.
- Log and investigate relevant denied or unusual connections.
- Back up configurations securely.
- Restrict administrative access.
- Test changes before deploying them broadly.
- Keep documentation of rule purpose, owner, and review date.

**Key takeaway:** Firewall security depends on correctly scoped rules, regular review, secure administration, and a broader defense-in-depth strategy.

---

## Module 5: Endpoint Security

### 5.1 What is an endpoint?

An **endpoint** is a device that connects to a network, such as a laptop, desktop, smartphone, tablet, server, or supported networked device.

**Endpoint security** combines technologies, policies, and procedures to protect these devices against malware, unauthorized access, data loss, and other threats.

### 5.2 Why endpoint security matters

Endpoint security supports:

- **Cyberattack prevention:** Blocking or detecting malware, ransomware, and suspicious behavior.
- **Data loss prevention:** Reducing unauthorized disclosure or transfer of sensitive information.
- **Network protection:** Preventing compromised devices from becoming stepping stones to other systems.
- **Visibility and control:** Tracking software, hardware, and security status.
- **Incident response:** Investigating, isolating, and remediating affected endpoints.
- **Policy enforcement:** Applying security requirements consistently.

### 5.3 Endpoint security technologies

| Technology | Purpose |
|---|---|
| Antivirus / anti-malware | Detects and blocks known or suspicious malicious software |
| Host firewall | Controls network connections to and from an endpoint |
| Intrusion detection and prevention | Identifies or blocks suspicious activity, depending on the product |
| Data loss prevention (DLP) | Helps prevent unauthorized transfer of sensitive data |
| Endpoint encryption | Protects stored data if a device or drive is lost or stolen |
| Patch management | Keeps operating systems and applications updated |
| Device and application control | Restricts unauthorized peripherals and software |
| Behavioral analytics | Detects deviations from normal behavior |
| Endpoint Detection and Response (EDR) | Collects endpoint telemetry and supports threat investigation and response |

These controls complement one another. For example, antivirus alone may not detect every sophisticated intrusion, while EDR provides broader visibility into endpoint activity.

### 5.4 Endpoint security best practices

- Install updates promptly.
- Use strong, unique passwords and MFA.
- Enforce least privilege.
- Encrypt sensitive data and device storage.
- Maintain tested backups.
- Train users to recognize phishing and suspicious attachments.
- Use EDR or equivalent monitoring where appropriate.
- Restrict unapproved software and removable media.
- Audit configurations and access rights.
- Isolate compromised devices during incident response when appropriate.

### 5.5 Microsoft endpoint security products mentioned in the course

| Product | Main role |
|---|---|
| Microsoft Defender Antivirus | Malware protection on supported devices |
| Microsoft Defender Firewall | Controls endpoint network traffic |
| Microsoft Intune | Cloud-based endpoint and device management |
| Microsoft Sentinel | Cloud-based SIEM and security analytics platform |
| Microsoft Secure Score | Assesses security posture and recommends improvements across supported Microsoft environments |

**Terminology note:** Microsoft Sentinel was previously known as Azure Sentinel. Secure Score is a posture-measurement and recommendation feature, not an endpoint protection product in itself. Product availability and licensing vary by environment.

### 5.6 EDR versus antivirus

Antivirus primarily focuses on detecting and blocking malware. EDR collects broader endpoint activity and helps security teams investigate suspicious behavior, trace attack sequences, and respond to incidents.

For example, an antivirus product might block a malicious executable, while EDR may help investigate how it arrived, which processes it launched, and whether related accounts or systems were affected.

**Key takeaway:** Endpoint security protects the devices through which users access organizational systems and data. It works best alongside network security, identity controls, monitoring, and backups.

---

## Module 6: Keeping Applications and Operating Systems Updated

### 6.1 What is a zero-day vulnerability?

A **zero-day vulnerability** is a software flaw for which a fix is not yet available to the affected organization or users. The exact term is sometimes used differently across security reporting, but the key concern is that defenders may have little or no time to patch before exploitation.

A zero-day vulnerability is not the same as a zero-day exploit:

- **Vulnerability:** The underlying weakness.
- **Exploit:** A technique or code that takes advantage of a weakness.
- **Patch:** A software update that fixes a vulnerability.
- **Mitigation:** A measure that reduces risk, potentially before a patch is available.

### 6.2 Reducing zero-day risk

- Monitor credible vendor security advisories.
- Use threat detection and behavior monitoring.
- Restrict unnecessary applications and services.
- Apply available patches promptly.
- Temporarily disable or isolate a vulnerable component when appropriate.
- Maintain an incident-response plan.
- Keep tested backups and recovery procedures.
- Review logs for evidence of exploitation.

Patching is essential, but it cannot always prevent exploitation of a previously unknown flaw. Compensating controls remain important.

### 6.3 Historical examples

**WannaCry (2017):** Ransomware exploited a known Windows vulnerability in systems that had not been appropriately patched. The incident demonstrated the risks of delayed security updates and exposed legacy systems.

**Heartbleed (2014):** A serious flaw in certain versions of OpenSSL could expose sensitive memory contents from vulnerable systems. Depending on the circumstances, this could include secrets such as credentials or cryptographic material. Heartbleed was not simply an operating-system update problem; affected OpenSSL software also needed remediation.

### 6.4 Updating Windows

The course describes these steps for older Windows versions:

1. Open Settings.
2. Select Update & Security.
3. Open Windows Update.
4. Select Check for updates.
5. Install applicable updates and restart if required.

On Windows 10 and Windows 11, menu names and locations differ. Use the Windows Settings search for **Windows Update** if the course's menu path does not match your version.

Keep applications updated separately when they are not serviced through Windows Update.

### 6.5 How Windows Update works

The course describes the Windows Update Orchestrator as checking configured update sources, determining which updates apply, downloading updates, and coordinating installation and restarts.

Delivery Optimization can help manage bandwidth by coordinating how update content is downloaded.

In managed organizations, update sources and policies may be controlled by administrators. Updates should be deployed according to an appropriate maintenance and validation policy.

### 6.6 Updating macOS

The course describes App Store updates for applications and operating-system updates through Software Update.

On recent macOS versions:

1. Open System Settings.
2. Select General.
3. Select Software Update.
4. Install available updates or configure automatic updates as appropriate.

Applications obtained outside the App Store may use their own update mechanisms.

### 6.7 Updating Ubuntu and Debian-based Linux

```bash
sudo apt update
sudo apt upgrade
```

- `apt update` refreshes local package metadata.
- `apt upgrade` installs available package upgrades within its normal dependency-handling behavior.

Review proposed changes before confirming them, particularly on production systems.

### 6.8 Updating Fedora and related systems

The course provides:

```bash
sudo dnf check-update
sudo dnf upgrade
```

`dnf check-update` checks for available package updates. A nonzero exit status can indicate that updates are available, rather than necessarily indicating a failure. `dnf upgrade` installs applicable updates.

Commands and maintenance procedures can differ by distribution and version. Consult the documentation for the operating system in use.

### 6.9 A professional patch-management workflow

1. **Inventory:** Identify devices, operating systems, applications, and versions.
2. **Assess:** Prioritize vulnerabilities according to severity, exposure, and exploitation evidence.
3. **Test:** Validate important updates on representative systems when feasible.
4. **Deploy:** Roll out updates using a controlled process.
5. **Verify:** Confirm installation and required restarts.
6. **Monitor:** Check for compatibility issues and security incidents.
7. **Document:** Record patch status, exceptions, and unresolved risks.
8. **Recover:** Maintain rollback or recovery plans appropriate to the system.

Critical security updates may require expedited handling rather than waiting for a normal maintenance cycle.

**Key takeaway:** Updating software reduces exposure to known vulnerabilities. Effective patch management combines timely updates, verification, risk-based prioritization, and recovery planning.

---

## Module 7: How the Six Topics Work Together

Consider an organization that discovers unusual outbound traffic from an office laptop.

1. **Traffic analysis:** Wireshark or tcpdump helps examine the relevant network connections.
2. **APT investigation:** Analysts look for signs of persistence, unusual authentication, privilege escalation, or data exfiltration.
3. **IoT assessment:** Investigators check whether an insecure connected device could have provided an entry point.
4. **Firewall review:** Administrators examine permitted outbound connections and segmentation controls.
5. **Endpoint response:** Security staff inspect the laptop, use EDR telemetry, and isolate the device if necessary.
6. **Patch management:** The organization identifies and updates vulnerable software, then verifies that the initial weakness has been addressed.

This example illustrates **defense in depth**: multiple security controls work together so that a failure in one layer does not automatically compromise the entire organization.

## Module 8: Revision Questions

Try answering these without looking at the notes.

1. What is a man-in-the-middle attack?
2. How does Wireshark differ from tcpdump?
3. What distinguishes an APT from a brief, opportunistic attack?
4. Name four warning signs that may indicate an APT.
5. How did the Mirai botnet exploit insecure IoT devices?
6. Why is network segmentation useful for IoT security?
7. How does stateful inspection differ from basic packet filtering?
8. Why is NAT not a substitute for a firewall?
9. What is the difference between antivirus and EDR?
10. What is the purpose of endpoint encryption?
11. What is the difference between a vulnerability, an exploit, and a patch?
12. What do `apt update` and `apt upgrade` do?
13. Why can an unusual network connection be an indicator without being proof of compromise?
14. Describe a safe, authorized workflow for investigating suspicious traffic.

## Answer Key

1. An attacker intercepts or potentially manipulates communications between two parties.
2. Wireshark provides a graphical analysis interface; tcpdump is a command-line packet capture and analysis tool.
3. An APT is generally targeted and sustained, often seeking persistent access or strategic information.
4. Examples include unusual outbound traffic, unauthorized privilege changes, unknown processes, abnormal logins, disabled security tools, and recurring malware.
5. It infected vulnerable IoT devices, often using default credentials, and used the resulting botnet for DDoS attacks.
6. It limits unnecessary communication between IoT devices and more sensitive systems, reducing the potential spread of compromise.
7. Stateful inspection tracks active connections; basic packet filtering mainly evaluates packet-header information against rules.
8. NAT translates addresses but does not by itself establish a complete security policy.
9. Antivirus focuses on malware detection and prevention; EDR provides broader telemetry, investigation, and response capabilities.
10. It helps prevent unauthorized reading of stored data if a device or drive is lost or stolen.
11. A vulnerability is a weakness, an exploit takes advantage of a weakness, and a patch fixes the underlying software flaw.
12. `apt update` refreshes package metadata; `apt upgrade` installs available package upgrades.
13. Legitimate software and administration can create unusual traffic, so evidence must be correlated and investigated.
14. Obtain authorization, capture only necessary traffic, preserve evidence, inspect relevant packets and logs, investigate endpoints, contain confirmed threats, and document findings.

## Glossary

- **APT:** Advanced Persistent Threat.
- **C2:** Command and control communications.
- **DDoS:** Distributed denial of service.
- **DLP:** Data loss prevention.
- **EDR:** Endpoint Detection and Response.
- **Firmware:** Software embedded in a device that controls its operation.
- **IDS/IPS:** Intrusion Detection System / Intrusion Prevention System.
- **IoT:** Internet of Things.
- **LAN:** Local Area Network.
- **MFA:** Multi-factor authentication.
- **MitM:** Man-in-the-middle.
- **NAT:** Network Address Translation.
- **NGFW:** Next-generation firewall.
- **Packet capture:** Recording network packets for analysis.
- **Patch management:** The process of assessing, deploying, and verifying software updates.
- **SIEM:** Security Information and Event Management.
- **TLS:** Transport Layer Security, which protects communications when correctly configured.
- **Zero-day:** A vulnerability or exploit associated with a flaw that defenders may not yet have a usable fix for.
