## Objective 3: Understand Terraform basics

**General getting started tips**
<p>
<details><summary>Write configuration</summary>
<p>

`Terraform {}` block contains TF settings, including providers. For each provider a `source` is also defined. Also set a version attribute (optional). 
<br>

`Provider` block configures the specified provider. Provider is a plugin used to create and manage your resources.
<br>

`Resources` block to define components in your infra. Can be physical or virtual component, like ec2 or an app. They have 2 strings before the block: resource type + resource name. 
(`aws_instance.app_server`). Contain arguments which are used to config the resource. Including machine sizes etc. 
</details>

<p>
<details><summary>Initialize the directory</summary>
<p>

When you create new config / check existing config - you must initialize the directory with `terraform init`. 
When you do this, TF downloads and installs the providers defined in the config. 
TF downloads provider and installs it in a hidden subdirectory of your current working dir - named `.terraform`. TF also creates a lock file named `terraform.lock.hcl` - specifies exact provider versions used
</details>

<p>
<details><summary>Format and validate the configuration</summary>
<p>

Consistent formatting by using `terraform fmt`, auto updates configs for readability and consistency. Also prints out the files it modified. 
Use `terraform validate` to make sure config is syntactically valid and internally consistent.
</details>

<p>
<details><summary> Create infrastructure </summary>
<p>

To apply config, use  `terraform apply`. Before execution, TF prints out an execution plan - describing the actions TF will take. 

Terraform will now pause and wait for your approval before proceeding. If anything in the plan seems incorrect or dangerous, it is safe to abort here before Terraform modifies your infrastructure.

In this case the plan is acceptable, so type yes at the confirmation prompt to proceed. Executing the plan will take a few minutes since Terraform waits for the EC2 instance to become available.
</details>

<p>
<details><summary> Inspect state </summary>
<p>

After you apply your changes, TF wrote data in a file called `terraform.tfstate`. Stores the IDs and properties of resources it manages. State file is the only way TF can track which resources it manages. Also contains sensitive data, so use TFC/TFE or store remotely. 

Inspect the state file using `terraform show`.
<br>

Can use `terraform state list` to show list of resources in project state.
</details>

<p>
<details><summary> Change Infrastructure </summary>
<p>

The prefix `-/+` means that Terraform will destroy and recreate the resource, rather than updating it in-place
Terraform can update some attributes in-place - indicated with the `~` prefix.

TF prompts for approval of the execution plan before proceeding. Answer `yes` to execute the planned steps.
</details>


<p>
<details><summary> Destroy Infrastructure </summary>
<p>

To destroy managed resources use `terraform destroy `command. It’s the inverse of `terraform apply` - it terminates all resources specified in your TF state. Doesn’t touch non TF resources.
The `-` prefix indicates that the instance will be destroyed. Auto determines the order to destroy - just like apply. 
Answer `yes` to execute plan and destroy all the things.
</details>


<p>
<details><summary> Output </summary>
<p>

Using a file called `outputs.tf` - you can define outputs for your resources. Use the command `terraform output` to query outputs. Use these to connect TF projects with other parts of your infra.

LPT (which you knew already lol): Terraform loads all files in the current directory ending in `.tf`, so you can name your configuration files however you choose.
</details>

<p>
<details><summary> 	Store remote state and migrate to TFC </summary>
<p>

- TFC allows easy version, audit and collaborate on infra changes. When setting up TFC, you need to add a `cloud` block and replace `organization-name` with your TFC name.
- `terraform login`, then paste the API key into the terminal. 
- `terraform init`, to re-initialize config and migrate to TFC. Type, `yes` when prompted.
- Then delete local state file `rm terraform.tfstate`

<br>
Set workspace variables
<br>

You must configure your workspace with your AWS credentials to authenticate the AWS provider.
Navigate to your workspace in TFC and go to the workspace's Variables page. Under Workspace Variables, add your `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` as Environment Variables, making sure to mark them as "Sensitive".
</details>

<p>

**Providers**
<p>
<details><summary> Provider configuration  </summary>
<p>
Providers allow Terraform to interact with cloud providers, SaaS providers, and other APIs.

The name given in the block header is the local name of the provider to configure. This provider should already be included in a `required_providers` block.
Providers require their own configuration for regions, authentication etc.
The body between `{ }` contain config arguments for provider. Most arguments are defined by provider itself. 
<p>

There are also two "meta-arguments" that are defined by Terraform itself and available for all `provider` blocks:
- Alias: for using the same provider with different config for different resources. Main reason for this to support multiple regions for cloud platform. Provider block without alias is default config for that provider.  To reference alternate provider config, specify in this format: `provider_name.alias`
- Version (deprecated): The `version` meta-argument specifies a version constraint for a provider, and works the same way as the version argument in a `required_providers` block.
</details>
<p>

**Provider Requirements**
<p>
<details><summary> Requiring Providers </summary>
<p>

Each TF module must declare which provider it requires. Declared in a `required_providers` block.  The `required_providers` block must be nested inside the top-level terraform block. Consists of local name, source location, version constraint

```terraform
terraform {
  required_providers {
    mycloud = {
      source  = "mycorp/mycloud"
      version = "~> 1.0"
    }
  }
}
```

Each argument in the `required_providers` block enables one provider. The key determines the provider's local name (its unique identifier within this module), and the value is an object with the following elements:
- Source: the global source address for the provider you intend to use, such as `hashicorp/aws`.
- Version: a version constraint specifying which subset of available provider versions the module is compatible with.
</details>

<p>
<details><summary> Names and addresses </summary>
<p>

Each provider has two identifiers:
<br>
A unique source address, which is only used when requiring a provider. A local name, which is used everywhere else in a Terraform module.

- Local name: Module specific and assigned when requiring a provider, must be unique per-module. 
    TF configs always refer to provider using their local name. IE: resources from aws, all begin with `aws_instance` etc. 

- Source address: This is the providers global identifier. Also tells TF where to download it. 
    Consists of: `Hostname/Namespace/Type`. 

    - Hostname: Name of TF registry that distributes the provider. Defaults to: `registry.terraform.io`
    - Namespace: organizational namespace within the registry. Represents org that publishes the provider.
    - Type: Short name for platform or system the provider manages. Usually the providers preferred local name. 

The source address with all three components given explicitly is called the provider's fully-qualified address.
</details>

<p>
<details><summary> Version constraints  </summary>
<p>

-Each provider dependency you declare, should have `version` constraint in the version argument, so TF can select single version per provider. <br>
The version meta-argument specifies a version constraint for a provider, and works the same way as the version argument in a `required_providers` block. The version constraint in a provider configuration is only used if `required_providers` does not include one for that provider.
<br>

-If omitted, TF will accept any version of the provider. 
Dependency lock file: can be used to control TF and ensure it always install same provider versions. Each module should at least declare the minimum provider version it is known to work with, using the `>=` version constraint syntax:
</details>

<p>
<details><summary> Built in providers </summary>
<p>
One provider that is built into TF. It enables the `terraform_remote_state` data source.
It has a special provider source address, which is `terraform.io/builtin/terraform`
<p>
</details>

<p>
<details><summary> In house providers </summary>
<p>

Anyone can develop their own TF provider. Can be used to configure proprietary systems.
One option to distribute provider, to run an in-house private registry. 
Another option is to place provider plugins in directories via `filesystem mirrors`

All providers must have source address, that includes hostname of registry - but that doesn’t actually need to provide an actual registry service. For in house, you can use like a fake name.
</details>


**Terraform settings**
<p>

- TF block: Settings are gathered together into `terraform` blocks. Each TF block contains settings related to TF’s behavior. Inside a TF block, only constant values can be used. 

- Configuring TF cloud: Add a nested `cloud` block in the terraform block.
  Using TFC through command line is called `CLI-driven run workflow`. When you use CLI workflow, TF plan/apply are remotely executed in TFC environment by default. This lets you use TFC with CLI workflow. 

- Configuring TF backend: Nested `backend` block configures which state backend TF should use. Backend defines where TF stores its state data files. 
  By default, TF uses backend called `local` - which stores state on a local disk. Don't need to configure a backend, when using TFC (If I have a `cloud block`, I cannot have a `backend block`.)


**Backend configuration**
<p>
<details><summary> Credentials and sensitive data </summary>
<p>

Backends store state in a remote service, which allows multiple people to access it. Accessing remote state generally requires access credentials, since state data contains extremely sensitive information. 
Terraform writes the backend configuration in plain text in two separate files.

1) The `.terraform/terraform.tfstate` file contains the backend configuration for the current working directory.
2) All plan files capture the information in `.terraform/terraform.tfstate` at the time the plan was created. This helps ensure Terraform is applying the plan to correct set of infrastructure.
</details>

<p>
<details><summary> Initialization </summary>
<p>

When you change a backend's configuration, you must run `terraform init` again to validate and configure the backend before you can perform any plans, applies, or state operations. <br>

After you initialize, Terraform creates a `.terraform/` directory locally. This directory contains the most recent backend configuration, including any authentication parameters you provided to the Terraform CLI. Do not check this directory into Git, as it may contain sensitive credentials for your remote backend. <br>

The local backend configuration is different and entirely separate from the `terraform.tfstate` file that contains state data about your real-world infrastruture. Terraform stores the `terraform.tfstate` file in your remote backend.

</details>

<p>
<details><summary> Partial configuration </summary>
<p>

- Don’t need to specify every required argument in backend config. When some or all arguments are omitted, it’s called: partial config. With a partial config, the remaining TF config arguments must be provided:
    - File: config file may be specified with the `init` command. Use the `-backend-config=PATH` option when running `terraform init`. 
    - Command line key/value pairs: This isn’t recommended for secrets. Use the `-backend-config=”KEY-VALUE”` when running `terraform init`.
    - Interactively: TF will ask you for the required values and prompt for an answer.

The final merged config is stored on disk in the `.terraform` directory. Should be Ignored from version control. It contains sensitive info. 
</details>


<p>
<details><summary> Command-line key/value pairs example </summary>
<p>

Example of passing partial config with command-line key/value pairs:

```terraform
$ terraform init \
    -backend-config="address=demo.consul.io" \
    -backend-config="path=example_app/terraform_state" \
    -backend-config="scheme=https"
```
</details>

<p>
<details><summary> Changing configuration </summary>
<p>

You can change your backend config at any time. Can change both config and backend type (S3 to consul). 
TF auto detects any changes in your config and request a `reinitialization`. TF will ask you want to migrate existing state to the new config. This allows you to easily switch from one backend to another.
If you're just reconfiguring the same backend, Terraform will still ask if you want to migrate your state. You can respond "no" in this scenario.
</details>

<p>
<details><summary> Unconfiguring a Backend </summary>
<p>

If you no longer want to use any backend, you can simply remove the configuration from the file. Terraform will detect this like any other change and prompt you to reinitialize.
As part of the reinitialization, Terraform will ask if you'd like to migrate your state back down to normal local state. Once this is complete then Terraform is back to behaving as it does by default.
</details>

**Provision Infrastructure**
<p>

<p>
<details><summary> title </summary>
<p>

</details>

<p>
<details><summary> title </summary>
<p>

</details>

<p>
<details><summary> title </summary>
<p>

</details>

<p>
<details><summary> title </summary>
<p>

</details>

