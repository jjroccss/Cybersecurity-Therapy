# SPIM Chapter 2 - Incident Response Framework

###### tags: #SPIM 

## Table of Contents
```toc
```

## Cyber Incident Response Life Cycle
- Preparation
- Detection and Analysis
- Containment/Eradication
- Recovery
- Post-Incident Activity

### Preparation
- Main goal is to prevent incidents by ensuring that systems, networks and applications are sufficiently secure in advance
- Designing the systems securely
- Implementing defence mechanisms
- Proactively looking for threats

### Detection and Analysis
- Collection and processing data from the system sources
- Classification of cyber incidents by types
- Prioritizing cyber incidents
- Managing incidents investigation
- Incident documentation

#### Signs of Incidents
- **Precursor**
	- Sign that an incident may occur in the future
- **Indication**
	- Sign that an incident occurred or may be occurring now

#### Incident Detection Techniques
- Feedback from customers
- Feedback from staff
- System logs and application logs
- Security events log
- Antivirus, antispyware alerts
- File integrity checking software
- Network-Based Intrusion Detection Systems
- Host-Based Intrusion Detection Systems
- Third-party monitoring service

### Analysis
- Inspecting alerts, raw events, application and system logs
- Working quickly to analyse and validate each incident following a pre-defined process and documenting each step taken
- Identifying the attacker and targets
- Assessing the risk to business processes
- Escalating to relevant personnel for further analysis
- Analyse if it is a targeted attack or in-the-wild threat

**Steps**
1. Correlation
	- Identify if detected activity is a characteristic of an intrusion attempt
2. Structural Analysis
	- Identify if intended target is vulnerable to the detected activity
3. Intrusion Path Analysis
	- Identify if intended target is vulnerable to detected activity
4. Behaviour Analysis
	- Identify if the detected ability is permitted by security policy

### Containment/Eradication
- Containing the threat
- If it is of spreading nature, prevent it from spreading further
- Prevent data leakage to the attacker (e.g. block communications, disabling accounts)
- Elimination of incident components
- Clean infected machines
- Disable breached accounts
- Evidence should be collected according to procedures that meet all applicable laws and regulations
- If alert if of a known issue act of predefined knowledge base and decision support

### Recovery
- Restore systems to normal operations
- Revert any limitations that were used for isolation
- Confirm that the systems are functioning normally
- If needed, restoring systems and/or files from clean backups

### Post-Incident Activity
- AR meetings with all involved parties after a major incident
	- Chain of events and times
	- Findings
	- Conclusions
	- Recommendations
		- What needs to be done
		- Proactive updates
- Updating the knowledge base with specific incident details for next time's handling
- Recognize security weaknesses and threats
- Identifying and mitigating all vulnerabilities that were exploited

## OODA Loop
- Decision-making occurs in a recurring cycle of **observe-orient-decide-act**
- An entity that can process this cycle quickly, observing and reacting to unfolding events more rapidly than an opponents can thereby get inside the opponent's decision cycle to gain an advantage

![](https://i.imgur.com/JL792pU.png)

## Conflict in Incident Handling
![](https://i.imgur.com/sez9jB0.png)

## Example of Detection, Analysis and Response  (Silicon SOC)
- **Detection:**
	- Silicon SOC received an corelated alert on `Exploit.url.MVX`
	- Host IP: `192.67.25.197`
	- Date: `26 Feb 2016 10.22:06 SGT`
	- <u>Behaviours observed:</u>
		- Suspicious JS Activity
		- Exploit Code Generic Detection
		- Evasion Behaviour
		- Exploit Code Activity
- **Analysis:**
	- The incident was analysed and compromised site that was surfed by the user is summarised
	- GET Request: `hxxp://super.koumas.net/boards/viewforum.php?f=a87&sid=59_huz5pl0p9v1d4a07t7zyzgp7g7tklnwawie7lofiwwz9xsb3-5txyiettf5k23nn2sqs3_36eujtvflqgx4z3nfl5lgj2q`
	- Referrer: `hxxp://news.geltuihameleon.info/hellomylittlepiggy/search/?keyword=84190`
	- Malicious Host: `hxxp://super.koumas.net`
- **Response Phase**
	- **Containment:**
		- Blacklist the malicious URLs
		- Disconnect and isolate the host from the network
		- Backup data
	- **Eradication and Recovery:**
		- Format and do a clean reinstallation of the operating system
		- Update to the latest version of flash player for all end hosts

## Incident Response Hierarchy of Capabilities
![](https://i.imgur.com/RMGV2rr.png)

## Security Operation Centre
- Centralised facility that collects information on threats and protect an organisation's IT systems against attacks
- **Examples:**
	- Internal Threats
	- External Threats
	- User Activities
	- Policy Violation
	- Systems Availability

### Typical SOC Staffing
![](https://i.imgur.com/eWeOjso.png)
### Processes in a SOC
![](https://i.imgur.com/IF6eaSR.png)

### Raw Logs to Correlated Security Events
1. Collect event logs in native device formats from multiple devices
2. Normalise data into a common format
3. Correlate data across multiple rules to drill down to critical events
4. Real-time monitoring and detection

## Need for Incident Response Plans
- Preventive activities based on the results of risk assessments can lower the number of incidents but not all incidents can be prevented
- Computer security incident is a violation or imminent threat of violation of computer security policies, acceptable use policies and standard security practices
- Incident response capability is necessary for:
	- rapidly detecting incidents
	- minimizing loss and destruction
	- mitigating the weaknesses that were exploited
	- restoring business services

### Incident Management Guidelines
- **Recommended Actions:**
	- Defining incident response policy and plan
	- Procedure planning for incident handling and reporting
	- Setting guidelines for communicating with internal or external groups regarding incidents
	- Selecting and staffing incident response team according to pre-defined structure and staffing model
	- Determining services provided by incident response team
	- Training plans for incident response team

### Benefits of Incident Response
- Responding to incidents systematically so that appropriate steps are taken
- Helping personnel to recover quickly and efficiently from security incidents, minimizing loss or theft of information and disruption of services
- Using information gathered during the incident handling can help better prepare for handling future incidents and provide stringer protection for systems and data
- Dealing properly with legal issues that may arise during incidents

### Information Security Incident Management Framework
- **Incident Response Services**
	- <u>Incident Response Policy</u>
		- Incident Response Procedure
		- Communication Guidelines
	- <u>Incident Response Team</u>
		- Staffing and Training
		- Relationship with the other groups

### Establishing an Incident Response Team
- Incident response team personnel
- Incident response team staffing
- Selection of team models
- Outsourcing contractor issues
- Dependencies within organizations

#### Incident Response Team Personnel
- **Team Manager and Duty Team Manager**
	- Regardless of which incident response model an organisation chooses, a single employee should be in charge of incident response
	- In a fully outsourced model, the team manager is responsible for overseeing and evaluating the outsourcer's work
	- In other models, this responsibility is achieved by having a team manager and a deputy team manager who assumes authority in the absence of the team manager
- Technical lead
- Team members with good technical and communication skills
- **Employees**
	- The organisation performs all of its incident response work with limited technical and administrative support from contractors
- **Partially Outsourced**
	- Organisations outsource portions of its incident response work
	- Most prevalent arrange is for the organisation to outsource 24/7 monitoring of intrusion detection sensors, firewalls and other security services provider
	- Some organisations perform basic incident response work in-house and call contractors to assist with handling incidents, particularly those that are more serious or widespread
	- The services are mostly performed by contractors are mostly:
		- computer forensics
		- advanced incident analysis
		- incident containment
		- eradication and vulnerability mitigation
- **Fully Outsourced**
	- The organisation completely outsources its incident response work typically to an onsite contractor
	- This model is most likely to be used when the organisation needs a full-time onsite incident response team but does not have time available qualified employees

#### Training
- Regular mock-up incident response exercise
- Walkthrough the incident response policy and procedures
- Attend external training courses
- Drafting new incident response procedures
- After major incident has been handled, organisation should hold a meeting to:
	- review how effective incident handling process was
	- identify necessary improvements to existing security controls and practices
- Meetings should be held periodically for lesser incidents
	- Information accumulated from all lessons learned meetings should be used to identify systemic security weakness deficiencies in policies and procedures
- Follow-up reports generated for each resolved incident can be important not only for evidentiary purposes but also for reference in handling future incidents and in training new incident response team members
- Incident database with detailed information on each incident that occurs can also be valuable source of information for incident handlers

### Selection of Team Models
#### Central Incident Response Team
- Single incident response team that handles incidents throughout the organisation
- Effective for small organisations and for large organisations with minimal geographic diversity in terms of computing resources

#### Disrupted Incident Response Teams
- An organisation has multiple incident response teams each responsible for handling incidents for a particular logical or physical segment of the organisation
- Effective for large organisations (e.g. one team per division) and for organisations with major computing resources at distant locations

#### Coordinating Team
- Incident response team provides guidance and advice to other teams without having authority over those teams

### Factors to be Considered when Selecting Team Models
- **Need for 24/7 Availability**
	- Larger and smaller organisations that support critical infrastructures usually need incident response staff to be available 24/7
- **Full-Time vs Part-Time Team Members**
	- Organisations with limited funding, staffing or incident response needs may have only part-time incident response team members
	- In this case, the incident response team can also be known as a volunteer fire department
- **Cost**
- **Staff Expertise**
- **Employee Morale**
	- Incident response work and on-call responsibilities of team members is very stressful
	- Many organisations struggle to find willing, available, experienced and properly skilled people to participate in 24-hour support
- **Organisational Structure**
	- If an organisation has three department that function independently, incident response may be more effective if each department has its own incident response team
	- Main organisation can host a centralised incident response entity that facilitates standard practices and communications among the teams

### Outsourcing Contractor Issues
- Sensitive information is revealed to the contractor
- Current and future quality of work of contractor
- Division of responsibilities
- Lack of organisation-specific knowledge
- Lack of correlation
	- If intrusion detection system records attempted attack against web server but the outsourcer has no access to the web, it may be unable to determine if the attack was successful
- Handling incidents at multiple locations
	- If outsourcer is offsite, consider where outsourcer is located and how quickly it can have an incident response team at any facility and the cost
- Maintaining incident response skills in-house
	- Organisations that completely outsource incident response should strive to maintain incident response skills in-house
	- As situation may arise where the outsourcer is unavailable

### Dependencies Within Organisations
- Incident response teams rely on the expertise, judgement and abilities of others, including:
	- Management
	- Information security
	- Telecommunications
	- IT support
	- Legal department
	- Public affairs and media relations
	- Human resources
	- Physical security and facility management
	- Business continuity planning