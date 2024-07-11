## Table of Contents

  - [Table of Contents](#Table\of\Contents)
          - [Objectives](#Objectives)
  - [CI/CD](#CI/CD)
    - [PPE - CI/CD Attacks](#PPE\-\CI/CD\Attacks)
    - [CI/CD Environment](#CI/CD\Environment)
    - [Automation Platforms](#Automation\Platforms)
    - [Indirect Poisoned Pipeline Execution](#Indirect\Poisoned\Pipeline\Execution)

## Table of Contents

          - [Objectives](#Objectives)
  - [CI/CD](#CI/CD)
    - [PPE - CI/CD Attacks](#PPE\-\CI/CD\Attacks)
    - [CI/CD Environment](#CI/CD\Environment)
    - [Automation Platforms](#Automation\Platforms)
    - [Indirect Poisoned Pipeline Execution](#Indirect\Poisoned\Pipeline\Execution)

###### Objectives
- Learn about pipeline poisoning attacks
- Understand how to secure CI/CD pipelines
- Introduction to secure software development lifecycles (SSDLC) & DevSecOps

GitLab is built around `git` a Distributed Verson Control System. 
- **Version Control System**: A CVS is the environment where you manage and track changes mades in the codebase. It makes it easier to collaborate with others and maintain the history and versioning of a project.

- **CI/CD Pipelines**: Pipelines automate the building, testing, and deployment processes. Pipelines ensure the code is consistently integrated, tested, and delivered to the specified environment (production or staging).
- **Security Scanning**: GitLab has a few scanning features, like incorporating static application security testing (SAST), dynamic application security testing (DAST), container scanning, dependency scanning etc. These tools help identify and mitigate security threats in code and infrastructure

## CI/CD
We mentioned CI/CD in the context of pipelines. CI/CD stands for continuous integration and continous delivery.

- Continuous Integration: CI refers to integrating code changes from multiple contributors into a shared repository (where code is tored in a VCS; you can think of it as a folder structure). In GitLab, CI allows dev's and engineers to commit code frequently, triggering automations that leads to builds and tests. This is what continuous integration is all about: ensuring that code changes and updates are continiuously validated, which reduces the likelihood of vulnerabilities when introducing security scans and test as part of the validation process (here, we start entering the remit of DevSecOps)
- Continous Deployment: CD automates code deployment to different environments. During SDLC, code travels to environments like sandbox and staging, where the tests and validations are performed before they go into the production environment. The production environment is where the final version of an app or service lives, which is what we, as users, tend to see. CD pipelines ensure the code is securely deployed consistently and as part of DevSecOps. Integrating security checks before deployment key to production.


### PPE - CI/CD Attacks
**Poisoned Pipeline Execution** involves compromising a component or stage in the SDLC. For this attack to work, it takes advantage of the trust boundaries established within the supply chain, which is extremely common in CI/CD, where automation is everywhere.



### CI/CD Environment

Often, developers or other end-users only see a limited portion of the CI/CD pipeline. Developers interact with Git on a daily basis, so it makes sense that CI/CD is most commonly associated with Git – although it only makes up a quarter of a typical CI/CD pipeline. The diagram below visualises the general segments of a pipeline: development, build, testing, and deployment. While these segments could be expanded and interchanged, all pipelines will follow a similar order.

![Diagram showing the CI/CD pipeline steps.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e73cca6ec4fcf1309f2df86/room-content/ec57dbc0548b41e39700b9ba597514a8.svg)  

In the previous task, we looked at a CI/CD environment that was self-contained in Git. In a more formal environment, segments of the pipeline may be separated out onto different platforms. Below is the CI/CD environment we'll be exploring in this room. You will notice the addition of Jenkins, a build platform and automation server. In the next section, we will explore Jenkins and discuss how these components interact and contribute to the pipeline.  

![Diagram showing a Jenkins agent is initiated by a pipeline in Jenkins, started from Gitea.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e73cca6ec4fcf1309f2df86/room-content/647536544e06313b910f707eb1e02003.svg)  

### Automation Platforms

Jenkins, along with many other applications, handles a pipeline's build segment. These platforms can be remote or local. For example, Travis CI is a remote build platform, whereas Jenkins is a local automation server.

These platforms rely on runners or agents to build a project on a pre-configured VM. One advantage of some automation platforms is that they can automatically create and configure build environments on demand. This allows building and testing in different environments without manual configuration or administration.




### Indirect Poisoned Pipeline Execution
If an attacker doesn't have direct write access (to a main-protected or branch-protected repository, for example), it's possible they have write access to other repositories that could **indirectly** modify the behaviour of the pipeline execution.

If an environment is employing a development pipeline, a configuration file must be defined for the steps the build system must take. If a repository contains all the necessary source and build files, and another repository contains the pipeline files, write permissions could differ between the two, resulting in an indirect PPE vulnerability. In this example, you can assume that the repository containing the source is not write-protected and the repository containing the pipeline is write-protected.


```makefile
stage('make') {
	steps {
		build() {
				sh 'make || true'
		}
	}
}
```


Navigate to [http://MACHINE_IP:3000](http://MACHINE_IP:3000/), the Gitea platform AntarctiCrafts uses for version control and development. Log in using the credentials `guest:password123`. When you have logged in successfully, you should see two repositories: **gift-wrapper** and **gift-wrapper-pipeline**. Navigate to [http://MACHINE_IP:8080](http://MACHINE_IP:8080/), the Jenkins platform AntarctiCrafts uses for building and automation. Log in using the credentials `admin:admin`. Once you have logged in successfully, you should see a project: **gift-wrapper-build**.


We should always follow the principle of least privilege, especially for systems in production. In this example, the user tracy is made in such a way that it has the same permissions as the admin. This gives the user more flexibility. However, it also runs the risk of misuse not only by the owner of the account but also by others who gain access to this account, as we did.  

To remove tracy from the sudo group, we use the following command: `sudo deluser tracy sudo`. To confirm removal from the sudo group, use `sudo -l -U tracy`.


Hardening SSH

The path to root has been made more complicated for the attacker, but that doesn't mean we should stop here. Attackers can be very creative in finding all sorts of ways to accomplish privilege escalation. Any additional layers will make it a lot harder for the bad actors to achieve their objectives.

Remember that as attackers, we were able to use SSH in this server to move laterally from a lower-level user. In light of this, we can disable password-based SSH logins so we can thwart the possibility of an SSH login via compromised plaintext credentials that are just lying around.

In the admin shell, go to the `/etc/ssh/sshd_config` file and edit it using your favourite text editor (remember to use sudo). Find the line that says `#PasswordAuthentication yes` and change it to `PasswordAuthentication no` (remove the # sign and change yes to no). Next, find the line that says `Include /etc/ssh/sshd_config.d/*.conf` and change it to `#Include /etc/ssh/sshd_config.d/*.conf` (add a # sign at the beginning). Save the file, then enter the command `sudo systemctl restart ssh`.

In the example below, the _egrep_ command shows what the lines within the file should look like. You can use the same command to see if you have successfully edited the file.

You should see the effect immediately when you log out of tracy in your attacking machine and try logging in again via SSH.

Example of Successful Edit of sshd_config

           `root@jenkins:~# egrep '^PasswordAuthentication|^#Include' /etc/ssh/sshd_config #Include /etc/ssh/sshd_config.d/*.conf PasswordAuthentication no root@jenkins:~# systemctl restart ssh`
        

Example of Successful Edit of sshd_config

           `root@AttackBox:~# ssh tracy@MACHINE_IP tracy@MACHINE_IP: Permission denied (publickey).`