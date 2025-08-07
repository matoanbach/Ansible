# 3.1 Exploring Modules and Content Collections
- In ansible 2.9 and ealier, thousands of modules were provided
- Since ansible 2.10, only essential modules are provided in the ansible-core package
- Additional modules are provided through content collections
- By organizing modules in content collections, responsibility for module development can be delegated to specific projects and companies
- Additional content collections can be installed from different sources:
  - Ansible Galaxy at https://galaxy.ansible.com
  - Ansible Automation Platform
  - Directly from tar archives

## ansible-navigator and Modules
- **ansible-navigator** gets modules from content collections using its container-based execution environments
- Using an execution environment means that you don't have to set up an Ansible control host, but just run required content from the execution environment.

## Using ansible-navigator Execution Environments
- To access RHEL-provided execution environments, first use a valid Red Hat account to log in to registry.redhat.io: `podman login registry.redhat.io`
- Next, use `ansible-navigator` images to download the execution environment container images, this will show the `ee-supported-rhel8` image (or a later version if that is available)
- Press Esc to exit the image list

## Understanding Module Names
- Modules that are provided through collections have a Full Qualified Collection Name (FQCN):
  - `ansible.builtin.service`
  - `ansible.posix.firewalld`
- Using an FQCN is recommended, as duplicate names may occur between different collections
- If no duplicate names occur, you may use short module names instead

# 3.2 Learning How to Use Modules
## Getting Help about Modules
- `ansible-doc` provides documentation about all aspects of Ansible
- Different types of items are documented, see `ansible-doc --help` for an overview
- If no type is specified, the module is used as the default type
- To get a list of available modules, use `ansible-doc -l`
- To show usage information about a specific item, use `ansible-doc [-t itemtype] item` 

## Getting Help with ansible-navigator
- `ansible-navigator` can also provide help about items
- `ansible-navigator doc -m stdout ansible.core.ping` provides help about the ping module
- Use `ansible-navigator --pp never` to tell navigator to never look for a newer container image
- You may find using `ansible-doc` faster

## History commands:
```bash
   21  podman login registry.redhat.io
   22  ansible-navigator images
   23  clear
   24  ansible-doc -t shell
   25  ansible-doc -l
   26  ansible-doc --help
   27  ansible-doc -t shell -l
   28  man ansible-doc
   29  ansible-doc -t filter password_hash
   30  ansible-doc -t keyword delegate_to
   31  ansible-doc user
   32  clear
   33  ansible-doc user
   34  ansible-doc ansible.posix.firewalld
   35  ansible-doc firewalld
   36  ansible-navigator doc -m stdout 
   37  ansible-navigator doc -m stdout ansible.core.ping
   38  ansible-navigator --pp never doc -m stdout ansible.core.ping
   39  ansible-navigator --pp never doc -m stdout user
   40  clear
   41  ansible-navigator --pp never doc -m stdout user
   42  ansible-navigator --pp never doc  user
```

# 3.3 Installing Content Collections
# 3.4 Using requirements.yml
# 3.5 ansible-navigator
# 3.6 Exploring Essential Modules
# 3.7 Idempotency
# 3.8 Using docs.ansible.com
