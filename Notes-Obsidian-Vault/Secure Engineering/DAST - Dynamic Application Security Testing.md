## Table of Contents

        - [Objectives](#Objectives)
  - [What is DAST?](#What\is\DAST?)
- [Spiders and Crawlers](#spiders\and\crawlers)
      - [Spidering an app with ZAP](#Spidering\an\app\with\ZAP)
    - [Using Spiders/Crawlers](#Using\Spiders/Crawlers)
        - [More Spiders](#More\Spiders)
    - [Dealing with logins](#Dealing\with\logins)

One of the many ways to test an application for vulnerabilities is to take a running instance and attack it just like an attacker would from the outside.

This process is known as Dynamic Application Security Testing (DAST) and will be the focus of the current room.

##### Objectives
- How can DAST be applied to find weaknesses in real-world applications
- Understand the pros and cons of using DAST instead of other techniques

## What is DAST?
The process of testing a running instance of a web application for weaknesses and vulnerabilities. It focuses on a black-box testing approach where vulnerabilities are found just like a regular attacker would find them. Simply put, DAST identifies vulnerabilities by trying to exploit them, either manually or through automated tools.


By testing the application from an outside perspective, we can abstract ourselves from its inner workings and focus on identifying vulnerabilities that an attacker would likely find. This means the results obtained from DAST will often point to vulnerabilities requiring prioritised attention as they are expected to be found without prior knowledge of the application.

- **Manual DAST:** A security engineer will manually perform tests against an application to check for vulnerabilities.
- **Automatic DAST:** An automated tool will scan the web application for vulnerabilities.

Both processes are complementary and can be used at different stages of the **Software Development Lifecycle (SDLC)**. Combining manual and automated tools will often yield the best results rather than relying on either separately.

Manual DAST will help us find weaknesses that an automated tool won't easily sport as they don't understand the application from a functional point of view.


DAST Pros and Cons

As with any other process used to find vulnerabilities in an application, DAST will have its advantages and disadvantages:

| Pros | Cons |
|---|---|
|- Finds vulnerabilities during runtime. This will include vulnerabilities that are specific to the deployment process, which can't be seen by analysing code alone.<br>- DAST will find vulnerabilities like HTTP Request Smuggling, cache poisoning, and parameter pollution that won't be found using SAST.<br>- DAST tools don't care about the programming language in use by your application. Since DAST is black-box testing, all apps are treated in a language-agnostic way.<br>- Reduced number of false positives as compared to SAST.<br>- DAST tools might be able to find some business logic flaws. The effectivity will depend on the tool, and shouldn't be taken as a replacement for manual testing.|- Code coverage isn't the best. DAST may not cover specific situations that will only be triggered by specific use cases in your application.<br>- Some vulnerabilities may be harder to find using DAST, as compared to static code analysis techniques.<br>- Some apps are difficult to be crawled. Modern applications heavily rely on javascript processing for the client-side. This makes it harder for DAST tools to traverse them.<br>- DAST scanners won't be able to tell you how to remediate some vulnerabilities in detail, since they have no idea of the underlying code.<br>- Some types of scans might take lots of time to finish.<br>- You need a running application to do the testing.|



DAST is often contexualised as a series of automated tests. However, we will explore how the manual process works to understand better what most tools fundamentally do. In general terms, a DAST tool will perform at least the two following tasks against the target website:
- Spidering/Crawling: The tool will navigate through the web app, trying to map the application and identify a list of pages and parameters that can be attacked
- Vulnerability Scanning: The tool will try to launch attack payloads against the identified pages and parameters. The user can typically customise the type of attacks to include only the ones relevant to the target application


Paid tools and Free tools (ZAP Proxy in this case) 




# Spiders and Crawlers
#### Spidering an app with ZAP
First step is to map all the resources in our target web application to identify the scope of our analysis. ZAP provides a spidering module that we can use to that end. A **Spider** will navigate to a starting page you provide and fundamentally explore the website by following all the links it can detect


### Using Spiders/Crawlers
Tools -> Spider
- Recurse: Spider the website for links recursively
- Spider Subtree Only: Limit the spidering to subfolders of the Starting Point URL
- Show Advanced Options: Enable the Advanced tab, where you can specify additional parameters to tune your spidering

##### More Spiders
To overcome the limitations of ZAP's regular spider, ZAP can leveragge a real browser like Firefox or Chrome to process any scripts attached to the website and retrieve the resulting HTML code. Instead of parsing links directly from HTTP responses, it will then do so from the result of a browser processing the website. This can be acheived by using the `AJAX spider`.


### Dealing with logins
So far, we have run a successful scan and found some vulnerabilities in the target application. Our application, however, still has vulnerabilities to uncover. If we manually navigate to our application, we will see an administration console that can only be accessed with proper credentials. Since ZAP knows not about these credentials, it won't be able to check anything that requires authentication.


API Descriptions

Several standards exist to document APIs, which generally include all the information developers would need to build proper requests and know what to expect as responses for each of the available endpoints. This is also all we need to start testing for security vulnerabilities.

ZAP can import APIs defined by **OpenAPI** (formerly **Swagger**), **SOAP** or **GraphQL**. Depending on the API you are testing, you might get one of these formats from the development team.

To give you a better idea of what these files contain, let's take a look at the one published on [http://MACHINE_IP:8081/swagger.json](http://machine_ip:8081/swagger.json), which follows the OpenAPI 2.0 standard. Inside the file, you will find a section called `paths`, where every single endpoint of the API is listed. For each of the endpoints, a complete list of the available parameters is presented. For example, here you have the part of the specification on how to interact with `/asciiart/{art_id}`:

```json
    "/asciiart/{art_id}": {
      "get": {
        "parameters": [
          {
            "description": "art id",
            "in": "path",
            "name": "art_id",
            "required": true,
            "type": "string"
          }
        ],
        "responses": {
          "default": {
            "description": "",
            "schema": {
              "$ref": "#/definitions/ASCIIArt"
            }
          }
        }
      }
    }
```

























