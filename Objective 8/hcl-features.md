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

- Resources: are the most important element in the Terraform language. Each resource block describes one or more infrastructure objects. 

<br>

- Data sources: allow Terraform to use information defined outside of Terraform, defined by another separate Terraform configuration, or modified by functions.
- The data source and name together serve as an identifier for a given resource and so must be unique within a module.
- Looks something like this: 

``` data "aws_ami" "example" {
  most_recent = true

  owners = ["self"]
  tags = {
    Name   = "app-server"
    Tested = "true"
  }
}
```

- Then you can reference that data resource using this format: `data.type.name.attribute `/ `data.aws_ami.example.id`. Pretty simple stuff.

</details>

<p>
<details><summary>8e: Use resource addressing and resource parameters to connect resources together</summary>
<p>

- A resource address is a string that identifies zero or more resource instances in your overall configuration. Takes this form: `[module path][resource spec]`
- A module path addresses a module within the tree of modules. It takes the form: `module.module_name[module index]`
- A resource spec addresses a specific resource instance in the selected module. It has the following syntax: `resource_type.resource_name[instance index]`
<br>

Named values. Each of these names is an expression that references the associated value:
- Resources: `<RESOURCE TYPE>.<NAME> `represents a managed resource of the given type and name.
- Input: `var.<NAME> `is the value of the input variable of the given name.
- Local: `local.<NAME>` is the value of the local value of the given name.
- Child Module Outputs : `module.<MODULE NAME>` is an value representing the results of a module block.
- Data Sources: `data.<DATA TYPE>.<NAME>` is an object representing a data resource of the given data source type and name. 
- Filesystem and Workspace Info: 
  - `path.module`, `path.root`,` path.cwd`, `terraform.workspace` 
</details>

<p>
<details><summary>8f: Use Terraform built-in functions to write configuration</summary>
<p>

- The Terraform language includes a number of built-in functions that you can call from within expressions to transform and combine values. EX: `max(5, 12, 9)`
<br>

- Use `templatefile` to dynamically generate a script:
  - You can use Terraform's `templatefile function` to interpolate values into the script at resource creation time. This makes the script more adaptable and re-usable. (User data script)
<br>

- Use `lookup` function to select AMI:
  - The `lookup function` retrieves the value of a single element from a map, given its key. (Map a region == specified ami. When you deploy, select the region and bam the selected ami is used)
<br>

- Use the `file` function:
  - The `file` function does not interpolate values into file contents; you should only use it with files that do not need modification. (SSH key for an instance. File just reads the contents )
<p>

- Use a conditional expression:
  - Conditional expressions select a value based on whether the expression evaluates to true or false.
  - ex: (condition)If the `name` variable is NOT empty (?)then	(true value)Assign the `var.name` value to the local value	(:)else	(false value)Assign `random_id.id.hex` value to the local value

```
  name  = (var.name != "" ? var.name : random_id.id.hex) 
```  
- Splat: The `splat` expression captures all objects in a list that share an attribute. The special `*` symbol iterates over all of the elements of a given list and returns information based on the shared attribute you define.
- Without the splat expression, Terraform would not be able to output the entire array of your instances and would only return the first item in the array.
</details>

<p>
<details><summary>8g: Configure resource using a dynamic block  </summary>
<p>

- Terraform provides the `dynamic` block to create repeatable nested blocks within a resource. A dynamic block is similar to the `for` expression. A dynamic block iterates over a child resource and generates a nested block for each element of that resource
- Something like this:
```
...
 dynamic "subnet" {
    for_each = var.subnets
    iterator = item   #optional
    content {
      name           = item.value.name
      address_prefix = item.value.address_prefix
...
```

vs:

```
...
subnet {
    name           = "snet1"
    address_prefix = "10.10.1.0/24"
  }
...
```
</details>

<p>
<details><summary>8h: Describe built-in dependency management (order of execution based)</summary>
<p>

- Terraform builds a dependency graph from the Terraform configurations, and walks this graph to generate plans, refresh state, and more. 
- Graph nodes:
  - Resource Node - Represents a single resource
  - Provider Configuration Node - Represents the time to fully configure a provider.
  - Resource Meta-Node - Represents a group of resources, but does not represent any action on its own
<br>

- TF will try to create a dependency graph based on your configuration and in cases where Terraform could not know implicitly that you can use a “depends_on” block to specify them explicitly.
</details>
