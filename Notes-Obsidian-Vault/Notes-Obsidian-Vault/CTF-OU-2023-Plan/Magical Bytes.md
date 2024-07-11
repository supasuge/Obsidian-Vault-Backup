## Table of Contents

        - [Resources](#Resources)

**Challenge Write-Up: "Beyond the Extension Lies the Truth"**

_**What are Magic Bytes?**_

Magic bytes (also known as file signatures) are the first few bytes at the beginning of a file. These bytes act like a unique fingerprint, identifying the file's true format regardless of its filename or extension.

_**What are they used for?**_

- **File Identification:** Most programs like image viewers, operating systems, and antivirus software use magic bytes to quickly and accurately determine the kind of file they're working with.
- **Forensics:** Forensic investigators use magic bytes to verify file types and look for potentially hidden or mislabeled files during analysis.

_**The Challenge**_

It seemed so simple... A basic image file to examine. However, things weren't adding up. The image wouldn't open correctly, or maybe it seemed entirely like the wrong kind of file altogether. The clue hinted towards the problem:

> "Beyond the Extension Lies the Truth: A deceitful label may attempt to fool. Trust the file's inner signature to guide your tool."

_**Solving the Challenge**_

2. **The Illusion of Extensions:** File extensions (`.jpg, .png, .pdf`, etc.) are meant to help us and our computers understand file types. However, they can be changed! Think of them like easily swapped labels rather than a definite identifier.
4. **Hex Editors: Our Forensic Tools:** To uncover a file's true type, we need to look deeper. Hex editors let us see the raw data of a file, including those magical bytes at the very beginning. Popular tools include:
    
    - `dhex`
    - `Bless` Hex Editor
    - `GHex`
    
6. **Seek the Signature:** Research common magic bytes for different file types. Here are some examples:
    - JPEG: `FF D8 FF`
    - PNG: `89 50 4E 47`
    - ZIP: `50 4B 03 04`
![[Pasted image 20240213070607.png]]
_**Key Takeaways**_
- Don't trust file extensions blindly! They can be misleading.
- Magic bytes offer a reliable way to determine a file's true identity.
- Hex editors are powerful tools for peeking "under the hood" of files.
`GrizzCTF{89504e470d0a}`
##### Resources
- [Magic Bytes file signatures](https://en.wikipedia.org/wiki/List_of_file_signatures)
![[Pasted image 20240213072715.png]]
- [Magic Bytes](https://www.netspi.com/blog/technical/web-application-penetration-testing/magic-bytes-identifying-common-file-formats-at-a-glance/)









