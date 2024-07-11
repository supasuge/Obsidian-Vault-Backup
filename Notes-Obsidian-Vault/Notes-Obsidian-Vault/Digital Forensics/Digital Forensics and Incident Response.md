## Table of Contents

    - [What is DFIR?](#What\is\DFIR?)
  - [Basic concepts of DFIR](#Basic\concepts\of\DFIR)
      - [Artifacts](#Artifacts)
      - [Evidence Preservation](#Evidence\Preservation)
      - [Chain of custody:](#Chain\of\custody:)

### What is DFIR?
This field covers the collection of forensic artifacts from digital devices such as computers, media devices, and smartphones to investigate an incident. This field helps security professional identify footprints left by an attacker when a security incident occurs, use them to deterine the extent of compromise in an environment, and restire the environment to the state it was before the incident occurred.

- Finding evidence of attacker activity in the network and sifting false alarms from actual incidents.
- Robustly removing the attacker,  so their foothold from the network no longer remains.
- Identifying the extent and timeframe of a breach. This helps in communicating with relevant stakeholders.
- Finding the loopholes that led to the breach. What needs to be changed to avoid the breach in the future?
- Understanding attacker behavior to pre-emptively block further intrusion attempts by the attacker.
- Sharing information about the attacker with the community.

## Basic concepts of DFIR
#### Artifacts
**Artifacts** are pieces of evidence that point to an activity performed on a system. When performing DFIR, artifacts are collected to support a hypotheses or claim about attacker activity. 

#### Evidence Preservation
When performing DFIR, we must maintain the integrity of the evidence we are collecting. For this reason, certain best practices are established in the industry. We must note that any forensic analysis contaminates the evidence. Therefore evidence that is collected must be write-protected. Then, a copy of the write-protected evidence is used for analysis.

#### Chain of custody:

Another critical aspect of maintaining the integrity of evidence is the chain of custody. When the evidence is collected, it must be made sure that it is kept in secure custody. Any person not related to the investigation must not possess the evidence, or it will contaminate the chain of custody of the evidence. A contaminated chain of custody raises questions about the integrity of the data and weakens the case being built by adding unknown variables that can't be solved. For example, suppose a hard drive image, while being transferred from the person who took the image to the person who will perform the analysis, gets into the hands of a person who is not qualified to handle such evidence. In that case, we can't be sure if he dealt with the evidence correctly and hence didn't contaminate the evidence with his activity.




