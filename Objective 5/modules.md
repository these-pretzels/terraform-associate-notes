## Objective 5: Interact with Terraform modules

<p>
<details><summary>5a: General module notes </summary>
<p>

- What the hell is a module:
    - A Terraform module is a set of Terraform configuration files in a single directory. Even a simple configuration consisting of a single directory with one or more `.tf` files is a module.

- Terraform commands will only directly use the configuration files in one directory, which is usually the current working directory.
<br>

- What are they used for:
    - Organize configuration
    - Encapsulate configuration - Encapsulation can help prevent unintended consequences, such as a change to one part of your configuration accidentally causing changes to other infrastructure
    - Re-use configuration
    - Provide consistency and ensure best practices
    - Self service - Modules make your configuration easier for other teams to use. 

</details>

<p>
<details><summary>5a: Module best practices </summary>
<p>

- Name your provider `terraform-<PROVIDER>-<NAME>`. You must follow this convention in order to publish to the Terraform Cloud or Terraform Enterprise module registries
- Start writing your configuration with modules in mind.
- Use local modules to organize and encapsulate your code. 
- Use the public Terraform Registry to find useful modules. 
- Publish and share modules with your team.
</details>

<p>
<details><summary> 5a: Contrast module source options </summary>
<p>

- Find modules: Verified modules are reviewed by HashiCorp to ensure stability and compatibility. Every page on the registry has a search field for finding modules.
- Using modules: The syntax for specifying a registry module is `<NAMESPACE>/<NAME>/<PROVIDER>`. For example: `hashicorp/consul/aws.` 
- Private Registry Module Sources: Private registry modules have source strings of the form `<HOSTNAME>/<NAMESPACE>/<NAME>/<PROVIDER>`. This is the same format as the public registry, but with an added hostname prefix.
</details>

<p>
<details><summary> 5b: Interact with module inputs and outputs </summary>
<p>

- Because calling a module cannot access their attributes directly. The child module can declare output values to export certain values to be accessed by the calling module. 
    - You could output a value called `instance_ids` then reference it using: `module.servers.instance_ids`

<br>

- Input: Using input variables with modules is similar to using variables in any Terraform configuration. 
- Output: Reference the output values using: `module.MODULE_NAME.OUTPUT_NAME`. You need to create an output in the root module and set it to the modules output. 
Something like this: 
```terraform
output "vpc_public_subnets" {
  description = "IDs of the VPC's public subnets"
  value       = module.vpc.public_subnets
}
```
- When using a new module for the first time, you must run either `terraform init` or `terraform get` to install the module. When you run these commands, Terraform will install any new modules in the `.terraform/modules` directory
</details>


<p>
<details><summary> 5c: Describe variable scope within modules/child modules </summary>
<p>

- The label after the `variable` keyword is a name for the variable, which must be unique among all variables in the same module. 
- Optional arguments:
    - default: default value will be used if no value is set when calling the module or running Terraform.
    - type: allows you to restrict the type of value that will be accepted as the value for a variable. (sting, number, bool)
    - description: this specifies the input variable's documentation
    - validation: a block to define validation rules, usually in addition to type constraints.
    - sensitive: limits Terraform UI output when the variable is used in configuration. (plan / apply)
    - nullable: specify if the variable can be null within the module.

- TF loads variables in this order:

    - Environment variables
    - The `terraform.tfvars` file, if present.
    - The `terraform.tfvars.json` file, if present.
    - Any `*.auto.tfvars` or `*.auto.tfvars.json` files, processed in lexical order of their filenames.
    - Any `-var` and `-var-file` options on the command line, in the order they are provided. (This includes variables set by a Terraform Cloud workspace.)

- To call a child module
    - Modules are called from within other modules using module blocks.
    - The label after the `module` keyword is a local name, which the calling module can use to refer.
    - All modules require a `source` argument. Its value is either the path to a local directory containing the module's configuration files, or a remote module source that Terraform should download and use. Don't forget to run `terraform init` afterwards.
    - Use the `version` argument in the `module` block to specify versions. It's recommended practice to use version. 
    - TF also has meta-arguments. `Count`, `for_each`, `providers`, `depends_on`
<br>

- To preserve existing objects, you can use refactoring blocks to record the old and new addresses for each resource instance. 

</details>

<p>
<details><summary> 5d: Discover modules from the public Terraform Registry </summary>
<p>

- Same notes as 5a above. lol.
- When using a new module for the first time, you must run either `terraform init` or `terraform get` to install the module. When you run these commands, Terraform will install any new modules in the `.terraform/modules` directory within your configuration's working directory. 
</details>

<p>
<details><summary> 5e: Defining module version	 </summary>
<p>

- Use the version argument in the module block to specify versions. It's recommended practice to use version. 
- Terraform will use the newest installed version of the module that meets the constraint; if no acceptable versions are installed, it will download the newest version that meets the constraint.
</details>


