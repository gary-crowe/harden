# harden
Ansible code to harden Azure image.
This code security hardens to V1.0.0 of the CIS standard.
Edit the user details within the PlaybookToRemediate.yml for your Azure user

The code will also run the OpenSCAP report and pull it back into the current directory as:
```bash
{{inventory_host}}_report.html
```
See ```example_report.html``` as an example.

G.Crowe 20/6/2022
