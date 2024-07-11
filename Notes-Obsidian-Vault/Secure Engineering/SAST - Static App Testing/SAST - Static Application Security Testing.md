## Table of Contents

  - [Manual vs Automated Code Review](#Manual\vs\Automated\Code\Review)
- [Code Review Approache](#code\review\approache)
  - [Overview](#Overview)
  - [Manual Code Review](#Manual\Code\Review)
  - [Automated Code Review](#Automated\Code\Review)
  - [Cost Comparison](#Cost\Comparison)
  - [Implementation Strategy](#Implementation\Strategy)
  - [Conclusion](#Conclusion)


Frequently testing the code for security bugs through a process known as code review. Code reviews consist of looking at the source code of an application to search for possible vulnerabilities using a white box approach. By having access to the code, the reviewer can guarantee a greater coverage of the application's functionalities and lower the time required to find bugs.

- Vulnerabilities introduced early in the development process if left unattended 

![[Pasted image 20231214233450.png]]
## Manual vs Automated Code Review
Often done through a combination of manual analysis and automated tools for the best results. Both approaches have advantages that need to be considered when deciding what is best at each stage of development

  
Certainly! I'll convert the provided information into detailed, concise Markdown-styled notes. This will include appropriate headings, bullet points, and emphasis where needed to make the content clear and easy to follow. Here's the converted content:

---

# Code Review Approache
## Overview
Code reviews are essential in the development lifecycle, combining both manual analysis and automated tools for optimal results. Each approach has its unique advantages and limitations.
## Manual Code Review

- **Advantages:**
    - Thorough analysis: Human evaluation provides precise results.
    - Contextual understanding: Humans can interpret nuances and complexities.
- **Limitations:**
    - Time-consuming: Reviewing thousands of lines of code manually is labor-intensive.
    - Potential for oversight: Fatigue can lead to missed vulnerabilities.

## Automated Code Review

- **Advantages:**
    - Efficiency: Rapid identification of common vulnerabilities.
    - Consistency: Uniform performance regardless of codebase size.
    - Cost-effective: Saves time and resources in the initial stages.
- **Limitations:**
    - Rule dependency: Misses vulnerabilities not defined in its ruleset.
    - Lack of depth: May not detect complex, nuanced vulnerabilities.

## Cost Comparison

- **Manual Review:** Generally more expensive due to the extensive time requirement for thorough analysis.
- **Automated Tools:** More cost-efficient, especially for identifying standard vulnerabilities quickly.
## Implementation Strategy

- **Early Development Lifecycle:**
    - Use automated tools for identifying and resolving common, straightforward vulnerabilities.
- **Periodic Manual Reviews:**
    - Conduct manual reviews at key project milestones.
    - Focus on complex vulnerabilities beyond the scope of automated tools.

## Conclusion

A balanced approach leveraging both automated tools and manual reviews at different stages ensures comprehensive code quality and security.