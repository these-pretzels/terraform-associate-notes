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

-TFC allows easy version, audit and collaborate on infra changes. When setting up TFC, you need to add a `cloud` block and replace `organization-name` with your TFC name.
-`terraform login`, then paste the API key into the terminal. 
-`terraform init`, to re-initialize config and migrate to TFC. Type, `yes` when prompted.
-Then delete local state file `rm terraform.tfstate`

<br>
Set workspace variables
<br>

You must configure your workspace with your AWS credentials to authenticate the AWS provider.
Navigate to your workspace in TFC and go to the workspace's Variables page. Under Workspace Variables, add your `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` as Environment Variables, making sure to mark them as "Sensitive".
</details>



<p>
<details><summary> title </summary>
<p>

</details>
