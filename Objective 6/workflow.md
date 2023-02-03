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

- Validate runs checks that verify whether a configuration is syntactically valid and internally consistent, regardless of any provided variables or existing state. 
- To initialize a working directory for validation without accessing any configured backend, use: `$ terraform init -backend=false`
- When you use the `-json` option, Terraform will produce validation results in JSON format. Highlighting errors in a text editor.

</details>

<p>
<details><summary> 6d: Generate and review an execution plan for Terraform (terraform plan) </summary>
<p>

- Creates an execution plan that you can preview your changes. When a plan happens, TF reads the current state file, compares current and prior state, proposes a set of changes.
- Can use `terraform plan -out=FILE` with apply in part of running TF in automation. 
- Plan modes: `-destroy`, which creates a plan to destroy all remote objects, and `-refresh-only`, which creates a plan to update Terraform state and root module output values.

</details>

<p>
<details><summary>6e: Execute changes to infrastructure with Terraform (terraform apply)	  	 </summary>
<p>

- The `terraform apply` command executes the actions proposed in a Terraform plan. 
- You can pass the `-auto-approve` option to instruct Terraform to apply the plan without asking for confirmation.
</details>

<p>
<details><summary>6f: Destroy Terraform managed infrastructure (terraform destroy)</summary>
<p>

- The `terraform destroy` command is a convenient way to destroy all remote objects managed by a particular Terraform configuration. AKA - burn all the things. Lol.
- Can also use `terraform plan -destroy` for a proposed destory changes.

</details>

