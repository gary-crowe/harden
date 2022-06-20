# harden
Ansible code to harden Azure image.
This code security hardens to V1.0.0 of the CIS standard.
Edit the user details within the PlaybookToRemediate.yml for your Azure user (azureuser)

The code will also run the OpenSCAP report and pull it back into the current directory as:
```bash
{{inventory_host}}_report.html
```
See ```example_report.html``` as an example.

## Running just the report.
If you only want to run the report after hardening, run the following:
```bash
ansible-playbook -vi "13.87.73.76," PlaybookToRemediate.yml --tags=report_only
```


G.Crowe 20/6/2022
