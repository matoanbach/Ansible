# 10.1 Ansible and Logging
## Troubleshooting without Logs
- Relevant Ansible processing information is often written to STDOUT while using the `ansible-playbook` command
- When using `ansible-navigator`, add the option `-m stdout`
- In the output, start by reading the PLAY RECAP section, and if anything is wrong, scroll up to read relevant output
- To increase verbosity of the output of Ansible commands, use one to four `-v` options while running the command: `ansible-playbook -vvv myplay.yml` 



# 10.2 Using the debug Module
# 10.3 Checking Playbooks for Issues
# 10.4 Using Check Mode
# 10.5 Using Modules for Troubleshooting and Testing
# 10.6 Troubleshooting Connectivity Issues
# Lesson 10 Lab: Troubleshooting Playbooks