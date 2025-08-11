# 10.1 Ansible and Logging
## Troubleshooting without Logs
- Relevant Ansible processing information is often written to STDOUT while using the `ansible-playbook` command
- When using `ansible-navigator`, add the option `-m stdout`
- In the output, start by reading the PLAY RECAP section, and if anything is wrong, scroll up to read relevant output
- To increase verbosity of the output of Ansible commands, use one to four `-v` options while running the command: `ansible-playbook -vvv myplay.yml` 

## Understanding Logging
- Ansible commands don't produce any logging, all output is written to STDOUT
- If you want to write output to a log file, use the `log_path:` setting to specify the name of the log directory to be used
  - Do no log to `/var/log`, as it requires `sudo` for write access
- `ansible-navigator` creates playbook artifact files in the directory that contains the playbook. These artifacts have detailed information containing all relevant information about the playbook execution
- The best way to use these artifact files is by accessing the logs from `ansible-navigator` in interactive mode

## Managing ansible-navigator Artifacts
- Artifact files are generated for every playbook run
- This will fill up the project directory if the playbook was run multiple times
- Also, artifact files contain all information that was used, which may contain sensitive information
- To skip artifact creation, configure ansible-navigator.yml to contain the following:

```yml
ansible-navigator:
    playbook-artifact:
        enable: false
```

# 10.3 Checking Playbooks for Issues
# 10.4 Using Check Mode
# 10.5 Using Modules for Troubleshooting and Testing
# 10.6 Troubleshooting Connectivity Issues
# Lesson 10 Lab: Troubleshooting Playbooks