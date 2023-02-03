## Objective 6: Navigate Terraform workflow	

<p>
<details><summary>6a: Describe Terraform workflow ( Write -> Plan -> Create ) </summary>
<p>

- Write - Author infrastructure as code. You write your code. 
    - Use branches to avoid collisions within your team. 
    - TFC: provide secure and centralized place to store input variables and state. After TFC configured, you just need API key to edit.

- Plan - Preview changes before applying. Plan to see the outcome of your code. 
    - Outputs allow peer review. Used with VCS and pull requests.
    - TFC: plan automatically run when PR created. Once completed status shows any changes you want to make - right from the PR.

- Apply - Provision reproducible infrastructure. Apply your code changes.
    - Once PR been approved and merged, review the final plan and compare with state file. 
    - TFC: after merge, you have concrete plan for review and approval. 

</details>


<p>
<details><summary>6a: General notes for workflow </summary>
<p>

- Terraform block { } contains settings, including providers. 
- For each provider, the `source` defines a hostname, namespace, and provider type. TF installs the provider from the `terraform registry` 
- The `provider` block configures the specified provider. Provider == plugin that TF uses to manage your resources.
- The `resources` block defines components of your infra. Can be ec2 or whatever.

</details>

<p>
<details><summary> 6b: Initialize a Terraform working directory (terraform init) </summary>
<p>

- The `terraform init` command initializes a working directory containing Terraform configuration files. It prepares the current working directory for use with TF.
- Backend init: when you run init with an already initialized backend, TF will update working directory to use new backend settings. 
    - To update backend config: must supply `-reconfigure` or `-migrate-state`
- Child module: To skip child module installation, use `-get=false`
- Plugins:
    - `-upgrade`- Upgrade all previously-selected plugins to the newest version
    - `-get-plugins=false` - Skip plugin installation
    - `-plugin-dir=PATH` - Force plugin installation to read plugins only from the specified directory

- Initializing a configuration directory downloads and installs the providers defined in the configuration. So, run this when you create a new config or checkout existing config from VCS. 
     - TF downloads the provider and installs it in a hidden directory: `.terraform` and creates a lock file: `.terraform.lock.hcl`

</details>

<p>
<details><summary>  6c: Validate a Terraform configuration (terraform validate)	 </summary>
<p>


</details>

<p>
<details><summary>  	 </summary>
<p>


</details>

<p>
<details><summary>  	 </summary>
<p>


</details>

<p>
<details><summary>  	 </summary>
<p>


</details>

