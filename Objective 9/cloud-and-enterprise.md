## Objective 9: Understand Terraform Cloud and Enterprise capabilities

<p>
<details><summary>9a: Describe the benefits of Sentinel, registry, and workspaces </summary>
<p>

Sentinel:
- Policies are rules that Terraform Cloud enforces on runs. You use the Sentinel policy language to define Sentinel policies. After you define policies, you must add them to policy sets that Terraform Cloud can enforce on workspaces.
- The most basic Sentinel task for Terraform is to enforce a rule on all resources of a given type.
- Note: Sentinel policies are available in the Terraform Cloud Team and Governance tier.
<br>

- Imports to define policy rules:
    - `tfplan` - Access a Terraform plan, which is the file created as a result of the terraform plan command.
    - `tfconfig` - Access a Terraform configuration. 
    - `tfstate` - Access the Terraform state
    - `tfrun` - Access data associated with a run in Terraform Cloud. 
<p>

Terraform Registry:
- Private registry works similarly to the public Terraform Registry and helps you share Terraform providers and Terraform modules across your organization. 
- Public modules and providers are hosted on the public Terraform Registry and Terraform Cloud can automatically synchronize them to an organization's private registry. 
- Private providers and private modules are hosted on an organization's private registry and are only available to members of that organization.
- You can create Sentinel policies to manage how members of your organization can use modules from the private registry.
<p>

Workspaces:
- When run locally, Terraform manages each collection of infrastructure with a persistent working directory, which contains a configuration, state data, and variables.
- Terraform Cloud manages infrastructure collections with workspaces instead of directories.

| Component      | Local Terraform | Terraform Cloud |
| -------------  | -------------   | -------------   |
| TF config      | On disk         | In linked version control repository, or periodically uploaded via API/CLI | 
| Variable values| As .tfvars files, as CLI arguments, or in shell environment | In workspace |
| State   | On disk or in remote backend | In workspace |
|Credentials and secrets | In shell environment or entered at prompts | In workspace, stored as sensitive variables |

- Terraform Cloud keeps some additional data for each workspace: State versions and run history
<p>

- Terraform Cloud workspaces are required. They represent all of the collections of infrastructure in an organization. You cannot manage resources in Terraform Cloud without creating at least one workspace.\
- Terraform CLI workspaces are associated with a specific working directory and isolate multiple state files in the same working directory, letting you manage multiple groups of resources with a single configuration. 
<br>

- Recommend that organizations break down large monolithic Terraform configurations into smaller ones, then assign each one to its own workspace and delegate permissions and responsibilities for them. Terraform Cloud can manage monolithic configurations just fine, but managing infrastructure as smaller components is the best way to take full advantage of Terraform Cloud's governance and delegation features.
</details>

<p>
<details><summary>9b: Differentiate OSS and Terraform Cloud workspaces</summary>
<p>

- Backend support multiple workspaces: AzureRM, Consul, COS, GCS, Local, OSS, Remote, S3 
- Terraform starts with a single, default workspace named default that you cannot delete.
- Within your Terraform configuration, you may include the name of the current workspace using the `${terraform.workspace}` interpolation sequence. 
- Example:
```terraform
resource "aws_instance" "example" {
  tags = {
    Name = "web - ${terraform.workspace}"
  }

  # ... other arguments
}
```

</details>

<p>
<details><summary>9c: Summarize features of Terraform Cloud </summary>
<p>

- Terraform Cloud is an application that helps teams use Terraform together. 
- It manages Terraform runs in a consistent and reliable environment.
- Easy access to shared state and secret data
- Access control for approving changes to infra
- Private registry for sharing modules
<br>

- The Business tier allows large organizations to scale to multiple concurrent runs, create infrastructure in private environments, manage user access with SSO, and automates self-service provisioning for infrastructure end users.
- TF enterprise is the same as TFC, but self hosted
<p>

- Terraform Cloud has three workflows for managing Terraform runs.
    - The UI/VCS-driven run workflow described below, which is the primary mode of operation.
    - The API-driven run workflow, which is more flexible but requires you to create some tooling.
    - The CLI-driven run workflow, which uses Terraform's standard CLI tools to execute runs in Terraform Cloud.

Summary, but you already knew most of this: 
- In the UI and VCS workflow, every workspace is associated with a specific branch of a VCS repo of Terraform configurations. Terraform Cloud registers webhooks with your VCS provider when you create a workspace, then automatically queues a Terraform run whenever new commits are merged to that branch of workspace's linked repository.

- Terraform Cloud also performs a speculative plan when a pull request is opened against that branch. Terraform Cloud posts a link to the plan in the pull request, and re-runs the plan if the pull request is updated.

- The Terraform code for a normal run always comes from version control, and is always associated with a specific commit.

</details>
