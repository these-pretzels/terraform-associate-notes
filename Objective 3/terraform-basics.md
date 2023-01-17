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
<details><summary> title </summary>
<p>

</details>


<p>
<details><summary> title </summary>
<p>

</details>
