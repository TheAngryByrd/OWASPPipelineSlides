- title : Security Automation in your Continuous Integration Pipeline
- description : Developers use unit tests and acceptances tests in continuous integration (CI) to find bugs early and often in a repeatable way. Security is an important part of any software development life cycle. So why not add security analysis tools to this pipeline? This talk will cover adding and using OWASP/glue, a framework made for running security analysis tools in CI.
- author : Jimmy Byrd
- theme : night
- transition : default

***

### Security Automation in your Continuous Integration Pipeline

![FsReveal](images/Continuous-Integration.png)

Jimmy Byrd

https://theangrybyrd.github.io/OWASPPipelineSlides

***

### Jimmy Byrd?
- [@jimmy_byrd](https://twitter.com/Jimmy_Byrd)
- [Github](https://github.com/theangrybyrd)
- Lead Developer at [Binary Defense Systems](https://www.binarydefense.com/)

![Binary Defense Systems](images/bds.png)




***

### Continuous what now?

---

### [Continuous Integration](http://martinfowler.com/articles/continuousIntegration.html)

> Continuous Integration is a software development practice where members of a team integrate their work frequently . . . Each integration is verified by an automated build (*including test*) to detect integration errors as quickly as possible. 
> 
> Martin Fowler

---

### Why?

To prevent: 

![Works on my machine](images/works-on-mine.jpg)

---

### Merging code

![Mergeing code](images/git-merge.gif)

---

### Continuous Integration 

1. Push
2. Build*
3. Test
4. Report

---

    ./build.sh

    ---------------------------------------------------------------------
    Build Report
    ---------------------------------------------------------------------
    Target           Duration
    ------           --------
    RestoreNpm       00:00:32.4737389
    PackWebAssets    00:00:03.7281990
    Linter           00:00:03.3159012
    Compile          00:00:22.2821302
    RunTests         00:00:04.9936549
    ScanCode         00:02:04.0223912
    Status:          Ok 
    ---------------------------------------------------------------------


---

### CI Tools

- [Teamcity](https://www.jetbrains.com/teamcity/)
- [Jenkins](https://jenkins.io/)
- [Gitlab](https://about.gitlab.com/gitlab-ci/)
- [Travis](https://travis-ci.org/)
- [AppVeyor](https://www.appveyor.com/)

*** 

### Software Testing 
![First test](images/first-test.gif)

---

### Automated testing hierarchy
![Automated Testing Pyramid](images/testing-pyramid.jpg)

***

### Why aren't we writing security tests?
![Baby Rhino Escape](images/baby-rhino.gif)

---

## [Security is an -ility](https://en.wikipedia.org/wiki/Non-functional_requirement)

Much like accessiblity, scalability, privacy


***

What are we trying to solve?

---

###  [Cross Site Scripting](https://www.owasp.org/index.php/Cross-site_Scripting_(XSS))
![Car fall](images/carfall.gif)

---

### Committing the production database password to source control
![Pearl freaking out](images/pearlfreak.gif)


---

### [Storing plain text passwords](https://www.owasp.org/index.php/Password_Storage_Cheat_Sheet)
![Mr Universe](images/muuniversefreak.gif)

---

### [Sql Injection](https://www.owasp.org/index.php/SQL_Injection)
![Lars Fire](images/larsfire.gif)

***

### Why don't we have both?
![Why not both](images/why-not-both.gif)


***

### [The Rugged Manifesto](https://www.ruggedsoftware.org/)

I am rugged and, more importantly, my code is rugged.

I recognize that software has become a foundation of our modern world.

I recognize the awesome responsibility that comes with this foundational role.

I recognize that my code will be used in ways I cannot anticipate, in ways it was not designed, and for longer than it was ever intended.

I recognize that my code will be attacked by talented and persistent adversaries who threaten our physical, economic and national security.

I recognize these things – and I choose to be rugged.

I am rugged because I refuse to be a source of vulnerability or weakness.

I am rugged because I assure my code will support its mission.

I am rugged because my code can face these challenges and persist in spite of them.

I am rugged, not because it is easy, but because it is necessary and I am up for the challenge.

***

### [OWASP/Glue](https://github.com/OWASP/glue)

>Glue is a framework for running a series of tools. Generally, it is intended as a backbone for automating a security analysis pipeline of tools.
>
> Github README

---

### Recently renamed

```
s/pipeline/glue
```

---

### Maintainers
- [Matt Tesauro](https://twitter.com/matt_tesauro)
- [Aaron Weaver](https://twitter.com/weavera)
- [Matt Konda](https://twitter.com/mkonda)

---

### Four simple concepts

- Mounters
- Tasks
- Filters
- Reporters

---

### What's in the box? (MOUNTERS)

- Docker
- File System
- Git
- ISO
- URL

---

### What's in the box? (TASKS)

- [ClamAV](https://www.clamav.net/)
- [Breakman](http://brakemanscanner.org/) (Ruby)
- [Bundle-Audit](https://github.com/rubysec/bundler-audit) (Ruby)
- [Checkmarx](https://www.checkmarx.com/) (Code)
- [DawnScanner](https://github.com/thesp0nge/dawnscanner) (Ruby)
- File Integrity Monitoring
- [FindSecurityBugs](https://find-sec-bugs.github.io/) (Java)
- [NodeSecurityProject](https://nodesecurity.io/) (Javascript)
- [OWASPDependencyCheck](https://github.com/jeremylong/DependencyCheck) (Java and .NET)
- [PMD Source Code Analyzer](https://pmd.github.io/) (Code)
- [RetireJS](https://retirejs.github.io/retire.js/) (Javascript)
- [Snyk](https://snyk.io/) (Javascript)
- [Zap](https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project)

---

### What's in the box? (FILTERS)

- Jira
- Zap

---

### What's in the box? (REPORTERS)

- CSV
- Jira
- Json
- Text

***

### Getting started
####Native

    [lang=bash]
    gem install owasp-glue

####Docker

    [lang=bash]
    docker pull owasp/glue
    docker run -i -t --entrypoint=/bin/bash owasp/glue

---

###Help 

    [lang=bash]
    glue --help
    
---

###Hello World!

    [lang=bash]
    glue -t eslint,retirejs https://github.com/OWASP/NodeGoat.git

---

###Hello World output

    Finding: NodeGoat.git
	Description: Package uglify-js-2.4.24 has known security issues
	Timestamp: 2016-06-24 14:43:35 +0000
	Source: {:scanner=>"RetireJS", 
             :file=>"owasp-nodejs-goat->swig->uglify-js-2.4.24", 
             :line=>nil, 
             :code=>nil}
	Severity: 0
	Fingerprint:  041c4f08bd5a3decc502217f15b7787b654b800e092ffadb939bd99e4e2cf26d
	Detail:  https://nodesecurity.io/advisories/48

***

### Tasks

Tools vs Labels

---

### Tools vs Labels

Have to go code spelunking

    [lang=bash]
    cd ./lib/glue/tasks
---

###Example from Brakeman.rb

    [lang=ruby]
    def initialize(trigger, tracker)
        super(trigger, tracker)
        @name = "Brakeman"
        @description = "Source analysis for Ruby"
        @stage = :code
        @labels << "code" << "ruby" << "rails"
    end

---

### Important pieces

- Name (without spaces) = Tool
- Labels = Labels

---

### All tools

    [lang=bash]
    av.rb:              "av"
    brakeman.rb:        "brakeman"
    bundle-audit.rb:    "bundleaudit"
    checkmarx.rb:       "checkmarx"
    dawnscanner.rb:     "dawnscanner"
    eslint.rb:          "eslint"
    fim.rb:             "fim"
    findsecbugs.rb:     "findsecuritybugs"
    nsp.rb:             "nodesecurityproject"
    owasp-dep-check.rb: "owaspdependencycheck"
    pmd.rb:             "pmd"
    retirejs.rb:        "retirejs"
    scanjs.rb:          "scanjs"
    sfl.rb:             "sfl"
    zap.rb:             "zap"

---

### Tools example

    [lang=bash]
    glue -t brakeman,eslint

This will run brakeman and eslint

---

### All Labels

    [lang=bash]
    av.rb:              "filesystem"
    brakeman.rb:        "code", "ruby", "rails"
    bundle-audit.rb:    "code", "ruby"
    checkmarx.rb:       "code"
    dawnscanner.rb:     "code"
    eslint.rb:          "code", "javascript"
    fim.rb:             "filesystem"
    findsecbugs.rb:     "code"
    nsp.rb:             "code"
    owasp-dep-check.rb: "code", "java", ".net"
    pmd.rb:             "code"
    retirejs.rb:        "code", "javascript"
    scanjs.rb:          "code", "javascript"
    sfl.rb:             "code"
    zap.rb:             "live"

---

### Labels example

    [lang=bash]
    glue -l ruby 

This will run brakeman and bundle-audit

***

### Building your own task/filter/reporter is pretty easy

***

### [OWASP/ZAP](https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project)
> The OWASP Zed Attack Proxy (ZAP) is one of the world’s most popular free security tools [...] It can help you automatically find security vulnerabilities in your web applications while you are developing and testing your applications.


---

### Getting started
#### Native 
[Download page](https://github.com/zaproxy/zaproxy/wiki/Downloads)

#### Docker

     [lang=bash]
    docker pull owasp/zap2docker-stable
    docker run -i -t --entrypoint=/bin/bash owasp/zap2docker-stable

---

#### GUI
    [lang=bash]
    docker run -u zap -p 8080:8080 -p 8090:8090 -i \
      owasp/zap2docker-stable zap-webswing.sh

    open "http://$(docker-machine ip default):8080/?anonym=true&app=ZAP"



#### Headless
    [lang=bash]
    docker run -u zap -p 8080:8080 -i owasp/zap2docker-stable \
      zap-x.sh -daemon -host 0.0.0.0 -port 8080

---

### Rest APIs
* [Java](https://github.com/zaproxy/zaproxy/releases)
* [Ruby](https://github.com/SUSE/owasp_zap)
* [Python](https://pypi.python.org/pypi/python-owasp-zap-v2.4)
* [NodeJS](https://www.npmjs.com/package/zaproxy)
* [.net](https://www.nuget.org/packages/OWASPZAPDotNetAPI/)
* [PHP](https://packagist.org/packages/zaproxy/php-owasp-zap-v2)


---



#### zap-cli

    [lang=bash]
    docker run -i owasp/zap2docker-stable zap-cli quick-scan --self-contained \
    --start-options '-config api.disablekey=true' http://target


---
### Results 

```
+----------------------------------+--------+----------+------------------------+
| Alert                            | Risk   |   CWE ID | URL                    |
+==================================+========+==========+========================+
| Cross Site Scripting (Reflected) | High   |       79 | http://web:4000/login  |
+----------------------------------+--------+----------+------------------------+
| Cross Site Scripting (Reflected) | High   |       79 | http://web:4000/signup |
+----------------------------------+--------+----------+------------------------+
| Cross Site Scripting (Reflected) | High   |       79 | http://web:4000/signup |
+----------------------------------+--------+----------+------------------------+
```

***


### First time on your code

![Bug report](images/bug-report.gif)

---

### Knowing is half the battle

![GI Joe](images/gi-joe.gif)

***

### Resources

- [OWASP Glue](https://www.owasp.org/index.php/OWASP_Glue_Tool_Project)
- [OWASP Glue Github](https://github.com/OWASP/glue)
- [Pipelines, DevOps and making things better - Matt Tesauro](https://www.youtube.com/watch?v=LfVhB3EiDDs)
- [Design Approaches for Security Automation - Peleus Uhley](https://www.youtube.com/watch?v=_IushM9Ng7A)
- [OWASP ZAP](https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project)