# 6.1 Understanding Facts
- Ansible Facts are variables that are automatically set and discovered by Ansible on managed hosts
- Facts contain information about hosts that can be used in conditionals
  - Available memory
  - IP Address
  - Distribution and distribution family
  - and much more
- Using facts allows you to perform actions only if certain conditions are true

## Managin Fact Gathering
- By default, all playbooks perform fact gathering before running the actual plays
- Use `gather_facts: no` in play header to disable
- Even if fact gathering is disabled, it can be enabled again by running the `setup` module in a task
- You can run fact gathering in an ad hoc command using the `setup` module
- To show facts, use the debug module to print the value of the `ansible_facts` variable
- Notice that in facts, a hierarchical relation is show where you can use the dotted format to refer to a specific fact.

## Troubleshooting Slow Fact Collection
- If fact collection is slow because you're working against lots of hosts, you can use a fact cache
- If you're experiencing slow fact gathering, ensure that host name resolution is set upp on all hosts
- Ensure that each managed host has an `/etc/hosts` file that allows for name resolution between all managed hosts

# 6.2 Using Multi-tier Variables
# 6.3 Understanding Dictionaries and Arrays
# 6.4 Defining Custom Facts
# 6.5 Understanding Variables
# Lab 6: Wokring with Facts