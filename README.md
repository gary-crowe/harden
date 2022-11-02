# harden
Ansible code to harden Azure image.
This code security hardens to V1.0.0 of the CIS standard.  There are 2 playbooks:
ciclvl1.yml : Hardens to CIS Level 1 Server 1
ciclvl2.yml : Hardens to CIS Level 1 Server 2

Edit the user details within the playbooks for your Azure user (azureuser)

The code will also run the OpenSCAP report and pull it back into the current directory as:
```bash
{{inventory_host}}_report.html
```
See ```example_report.html``` as an example.

This will use the current OPENSCAP reports from https://github.com/ComplianceAsCode/content/releases and not those installed as part of openscap.  You should check the current level (this code uses scap-security-guide-0.1.64-oval-5.10.zip).  If ot is later, then you will need to re-generate the ansible playbook using the following on a fresh install of the OS you plan to harden:

1.	Install openscap-scanner scap-security-guide 
2.	Download the latest zip file 
3.	```oscap info ./scap-security-guide-X.X.XX-oval-X.XX/ssg-rhel8-ds.xml```
4.	Choose the hardening you want (CIS 1, 2 etc)
```bash
oscap info --profile xccdf_org.ssgproject.content_profile_cis \  
                      ./scap-security-guide-0.1.62/ssg-rhel8-ds.xml

```
5.	Generate the report locally
```bash
oscap xccdf eval --profile xccdf_org.ssgproject.content_profile_cis \
    --results cis.xml ./scap-security-guide-0.1.62/ssg-rhel8-ds.xml
```
6.	Generate the ansible code
```bash
oscap xccdf generate fix --fix-type ansible --output PlaybookToRemediate.yml \
     --result-id "" cis.xml
```	
7.	Copy this code to your ansible server and remove AIDE, add login details etc as per the existing code here.

# Running the playbook.
You will need to have an ansible server and a virtual environment.
Instructs here are for RHEL.
1.	Subscribe your system
2.	Install python3.9 ```sudo dnf install python3.9```
3.	Install virtualenv ```sudo dnf install virtualenv```
4. 	Create your environment ```virtualenv -p python3.9 venv```
5.	Source that environment ```source ./venv/bin/activate```
6.	Install ansible ```pip install ansible```
7.	Run the required play ```ansible-playbook -vi "13.87.73.76," cislvl1.yml```

## Running just the report.
If you only want to run the report after hardening, run the following:
```bash
ansible-playbook -vi "13.87.73.76," PlaybookToRemediate.yml --tags=report_only
```


G.Crowe 02/11/2022
