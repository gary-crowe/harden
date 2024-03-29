# harden
Ansible code to harden Azure image.
This code security hardens to Linux 8 Benchmark™, v2.0.0, released 2022-02-23
This profile includes Center for Internet Security® Red Hat Enterprise Linux 8 CIS Benchmarks™ content.

There are 2 playbooks:
| PlayBook    | Description |
| ----------- | ----------- |
| ciclvl1.yml | CIS Red Hat Enterprise Linux 8 Benchmark for Level 1 - Server |
| ciclvl2.yml | CIS Red Hat Enterprise Linux 8 Benchmark for Level 2 - Server |

The code will harden, and also run the OpenSCAP report and pull it back into the current directory as: ```{{inventory_host}}_report.html ``` This will enable you to test the results for your garget system.  See ```example_report.html``` as an example of the generated report contents.

This code will use the current OPENSCAP reports from https://github.com/ComplianceAsCode/content/releases and not those installed as part of openscap.  You should check the current level (this code uses scap-security-guide-0.1.64-oval-5.10.zip).  If it is later, then you will need to re-generate the ansible playbook using the following on a fresh install of the OS you plan to harden:

1.	Install openscap-scanner scap-security-guide 
```bash
sudo dnf install openscap-scanner scap-security-guide -y
```
2.	Download the latest zip file 
```bash
wget https://github.com/ComplianceAsCode/content/releases/download/v0.1.64/scap-security-guide-0.1.64-oval-5.10.zip
```
3.	List the hardening protocols supported
```bash
oscap info ./scap-security-guide-X.X.XX-oval-X.XX/ssg-rhel8-ds.xml
```
4.	Choose the hardening you want (CIS 1, 2 etc)
```bash
oscap info --profile xccdf_org.ssgproject.content_profile_cis \
      ./scap-security-guide-0.1.62/ssg-rhel8-ds.xml
```
5.	Generate the report locally
```bash
oscap xccdf eval --profile xccdf_org.ssgproject.content_profile_cis --results cis.xml \
     ./scap-security-guide-0.1.62/ssg-rhel8-ds.xml
```
6.	Generate the ansible code
```bash
oscap xccdf generate fix --fix-type ansible --output PlaybookToRemediate.yml --result-id "" cis.xml
```	
7.	Copy this playbook to your ansible server and copy the ssg-rhel8-ds.xml file to the "files" folder.
8.	Edit your generated playbook and remove AIDE, add login details etc as per the existing code here.

# Running the playbook.
You will need to have an ansible server and a virtual environment.
Instructs here are for RHEL.
1.	Subscribe your system
2.	Install python3.9 : ```sudo dnf install python3.9```
3.	Install virtualenv :  ```sudo dnf install virtualenv```
4. 	Create your environment :  ```virtualenv -p python3.9 venv```
5.	Source that environment :  ```source ./venv/bin/activate```
6.	Install ansible :  ```pip install ansible```
7.	Run the required play :  ```ansible-playbook -vi "13.87.73.76," cislvl1.yml```

## Running just the report.
If you only want to run the report after hardening, run the following:
```bash
ansible-playbook -vi "13.87.73.76," PlaybookToRemediate.yml --tags=report_only
```


G.Crowe 02/11/2022
