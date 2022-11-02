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

This will use the current OPENSCAP reports from https://github.com/ComplianceAsCode/content/releases and not those installed as part of openscap.  You should check the current level.

# Running the playbook.
You will need to have an ansible server and a virtual environment.
Instructs here are for RHEL.
1.	Subscribe your system
2.	Install python3.9 ```sudo dnf install python3.9```
3.	Install virtualenv ```sudo dnf install virtualenv```
4. 	Create your environment ```virtualenv -p python3.9 venv
5.	Source that environment ```source ./venv/bin/activate```
6.	Install ansible ``` pip install ansible```
7.	Run the required play ```ansible-playbook -vi "13.87.73.76," cislvl1.yml```

## Running just the report.
If you only want to run the report after hardening, run the following:
```bash
ansible-playbook -vi "13.87.73.76," PlaybookToRemediate.yml --tags=report_only
```


G.Crowe 02/11/2022
