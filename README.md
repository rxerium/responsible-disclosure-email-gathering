# Responsible Disclosure Email Gathering

## Purpose

You may have found a vulnerability on a host but the said host/org does not have an active program on HackerOne or BugCrowd. If this is the case this workflow is for you as broadly it will crawl the website and look for any security related emails where you can submit your finding. 

## Prerequisites

- [Katana](https://docs.projectdiscovery.io/tools/katana/install)
- [Nuclei](https://docs.projectdiscovery.io/tools/nuclei/install)
- Active internet connection

## Process

This workflow will do the following:
1. Crawl the target host using Katana
2. The output from Katana will be parsed through to Nuclei
3. The nuclei template will run and fetch emails based on the below regex:
```yaml
   extractors:
      - type: regex
        part: body
        regex:
          - "(security|responsible-disclosure|responsibledisclosure|sec|csirt|cert|irt|vulnerability)@[A-Za-z0-9_-]+[.](com|org|net|io|gov|co|co.uk|com.mx|com.br|com.sv|co.cr|com.gt|com.hn|com.ni|com.au|com.cn)"
```

Note:
- Crawling is scoped to root domain name and all subdomains with the use of the `-fs rdn` flag


## Commands

### Single domain

```bash
echo domain.com | katana -fd rdn -silent | nuclei -t rd-extractor.yaml -stats -silent
```

### List of domains 

Create a new input.txt file with a list of domains - 1 per line

Then run the following:

```bash
cat input.txt | katana -fd rdn -silent | nuclei -t rd-extractor.yaml -stats -silent -o output.txt
```