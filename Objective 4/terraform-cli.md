## Objective 4: Use the Terraform CLI (outside of core workflow)

<p>
<details><summary>4a: Given a scenario: choose when to use terraform fmt to format code </summary>
<p>

The `terraform fmt` command is used to rewrite Terraform configuration files to a canonical and recommended format and style. 
Also ensures consistency within your codebase and can help troubleshoot invalid characters etc. 
</details>

<p>
<details><summary> 4b: Given a scenario: choose when to use terraform taint to taint Terraform resources </summary>
<p>

The `terraform taint` command informs Terraform that a particular object has become degraded or damaged. Terraform represents this by marking the object as "tainted" in the Terraform state, and Terraform will propose to replace it in the next plan you create.
<br>

For Terraform v0.15.2 and later, we recommend using the `-replace` option with `terraform apply` to force Terraform to replace an object even though there are no configuration changes that would require it.
</details>

<p>
<details><summary> 4c: Given a scenario: choose when to use terraform import to import existing infrastructure into your Terraform state </summary>
<p>

The `terraform import` command imports existing resources into Terraform. Just make sure the resource block exists in your codebase. 
</details>

<p>
<details><summary> 4d: Given a scenario: choose when to use terraform workspace to create workspaces </summary>
<p>

Terraform starts with a single, default workspace named `default` that you cannot delete. If you have not created a new workspace, you are using the default workspace in your Terraform working directory.
When you run terraform plan in a new workspace, Terraform does not access existing resources in other workspaces. These resources still physically exist, but you must switch workspaces to manage them.
EX: Refactor Monolithic Terraform Configuration
</details>

<p>
<details><summary> 4e: Given a scenario: choose when to use terraform state to view Terraform state </summary>
<p>

Rather than modify the state directly, the `terraform state` commands can be used in many cases instead. This command is a nested subcommand, meaning that it has further subcommands. 

The `terraform state list` command is used to list resources within a Terraform state.
The `terraform state show` command is used to show the attributes of a single resource in the Terraform state.
The `terraform refresh` command reads the current settings from all managed remote objects and updates the Terraform state to match.(deprecated command and will most likely never be used. Same thing happens when you TF plan/apply)
</details>

<p>
<details><summary> 4f: Given a scenario: choose when to enable verbose logging and what the outcome/value is </summary>
<p>

Terraform has detailed logs that you can enable by setting the `TF_LOG` environment variable to any value. Enabling this setting causes detailed logs to appear on `stderr`.
You can set `TF_LOG` to one of the log levels (in order of decreasing verbosity) `TRACE`, `DEBUG`, `INFO`, `WARN` or `ERROR` to change the verbosity of the logs.
To persist logged output you can set `TF_LOG_PATH` in order to force the log to always be appended to a specific file when logging is enabled.
</details>