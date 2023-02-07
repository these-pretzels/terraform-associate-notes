## Objective 8: Read, generate, and modify configuration


<p>
<details><summary> 8a: Demonstrate use of variables and outputs  </summary>
<p>

Inputs: 
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

Outputs:
- Several use cases:
    - Child module use outputs to expose resources to a parent module
    - Root module can use outputs to print values in cli, after running TF apply
    - When using remote state, root modules can be accessed by other configs using `terraform_remote_state data source`

- Need to use an `output` block to delcare output value. 

```terraform
output "instance_ip_addr" {
  value = aws_instance.server.private_ip
}
```
- You can use `precondition` blocks to specify guarantees about output data. 
- Output block arguments can incdlue: `description`, `sensitive`, and `depends_on`

<p>

- Terraform also supports `collection` variable types that contain more than one value. 
    - List: A sequence of values of the same type. You can use `slice()` func to get a subset of these variable lists. Think subnets, cidr blocks. 
    - Map: A lookup table, matching keys to values, all of the same type.
    - Set: An unordered collection of unique values, all of the same type.
</details>


<p>
<details><summary>8b: Describe secure secret injection best practice</summary>
<p>

- We recommend that you avoid placing secrets in your Terraform config or state file wherever possible, and if placed there, you take steps to reduce and manage your risk.
- Terraform can be used by the `Vault` administrators to configure Vault and populate it with secrets
<br>

- You can address long-lived credentials by storing in a vault secret engine. Then use a vault provider to get a short-lived cred, to do your task etc. 
- Using this can help reduce risk, management of long-lived creds, mgmt of role permissions rather than static creds. 
- Beware, if the apply take longer than TTL, or if the apply confirmation takes a while == you're going to run into issues.
</details>

<p>
<details><summary> 8c: Understand the use of collection and structural types </summary>
<p>

- "Complex type" is a type that groups multiple values into a single value. There are two categories of complex types: `collection types` (for grouping similar values), and `structural types` (for grouping potentially dissimilar values).
<br>

- Collection type  
    - Allows multiple values of one other type to be grouped together as a single value. The type of value within a collection is called its element type. 
    - 3 types of collection type: `list(..)`, `map(..)`, `set(..)`
<br>

- Structual type
    - A structural type allows multiple values of several distinct types to be grouped together as a single value. Structural types require a schema as an argument
    - 2 kinds of structual types: `object(..)`, `tuple(..)`

</details>

<p>
<details><summary> 8d: Create and differentiate resource and data configuration </summary>
<p>

</details>

<p>
<details><summary>  </summary>
<p>

</details>

<p>
<details><summary>  </summary>
<p>

</details>

<p>
<details><summary>  </summary>
<p>

</details>

<p>
<details><summary>  </summary>
<p>

</details>

<p>
<details><summary>  </summary>
<p>

</details>

<p>
<details><summary>  </summary>
<p>

</details>

