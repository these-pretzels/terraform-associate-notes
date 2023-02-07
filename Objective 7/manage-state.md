## Objective 7: Implement and maintain state


<p>
<details><summary>7a: Describe default local backend  </summary>
<p>

- By default, TF uses a backend called local, stored as a local file on disk. 

- Local config options: 
    - `path` - the path to the `tfstate` file, defaults to "terraform.tfstate"
    - `workspace_dir` - the path to non-default workspaces

- Backend types:    
    - The block label of backend block indicates which backend type to use. Arguments used in block body config where and how backend will store config state

</details>

<p>
<details><summary> 7b: Outline state locking	 </summary>
<p>

- If supported by backend, TF will lock state for all write operations. Prevents others from acquring lock and potentially corrupting state.
- It happens automatically and you won't see any message that it's happening. 
- You can disable state lock with `-lock` it's not recommended though.
</details>

<p>
<details><summary>7c: Handle backend authentication methods  </summary>
<p>

- Backend block: 
    - TFC manages state in your workspace. If you have a `cloud block` you can't have `backend` block
    - To config a backend, add a nested `backend` block in the top level `terraform` block. 
    - Remember, config can only provide one backend block. Backend block can't refer to named values.

- Credentials and sensitive data. TF writes backend config in plain text in 2 files:
    - `.terraform/terraform.tfstate` 
    - All plan files capture info in `.terraform/terraform.tfstate` at the time the plan was created

- Initialize: 
    - When you change a backend config you must run `terraform init` to validate and config the backend
    - After you initialize, TF creates `.terraform/` directory. Contains the most recent backend config. *don't check this into git, it contains sensitive info*
    - Local backend config is different than `terraform.tfstate` that contains real-world state data. TF stores `terraform.tfstate` in remote backend
</details>

<p>
<details><summary> 7d: Describe remote state storage mechanisms and supported standard backends </summary>
<p>

- Migrate to TFC. 
    - Run `terraform init` to initialize your configuration directory and download the required providers.
    - Setup workspace on TFC and add `cloud` block to the `main.tf` file. Replace `organization-name` with your TFC org name
    - `terraform login` to login to TFC account. 
    - Re-run `terraform init` to re-initialize and migrate state file to TFC. 
    - Delete local state file, `rm terraform.tfstate`
    - Set workspace variables in TFC portal (AWS creds etc)
</details>

<p>
<details><summary>7e: Describe effect of Terraform refresh on state  </summary>
<p>

- This command is deprecated lol. 
- The `terraform refresh` command reads the current settings from all managed remote objects and updates the Terraform state to match. Won't modify real objects, only modify Terraform state.
- Wherever possible, avoid using `terraform refresh` explicitly and instead rely on Terraform's behavior of automatically refreshing existing objects as part of creating a normal plan.

- Drift:
    - Run `terraform plan -refresh-only` to determine the drift between your current state file and actual configuration.
    - A refresh-only operation does not attempt to modify your infrastructure to match your Terraform configuration -- it only gives you the option to review and track the drift in your state file.

</details>

<p>
<details><summary> 7f: Describe backend block in configuration and best practices for partial configurations  </summary>
<p>

- Don’t need to specify every required argument in backend config. When some or all arguments are omitted, it’s called: partial config. With a partial config, the remaining TF config arguments must be provided:
    - File: config file may be specified with the `init` command. Use the `-backend-config=PATH` option when running `terraform init`. 
    - Command line key/value pairs: This isn’t recommended for secrets. Use the `-backend-config=”KEY-VALUE”` when running `terraform init`.
    - Interactively: TF will ask you for the required values and prompt for an answer.

The final merged config is stored on disk in the `.terraform` directory. Should be Ignored from version control. It contains sensitive info. 

</details>

<p>
<details><summary> 7g: Understand secret management in state files </summary>
<p>

- When using local state, state is stored in plain-text JSON files.
- Storing state remotely can provide better security. 
    - TFC: always encrypts state at rest and protects it with TLS in transit.
    - The S3 backend supports encryption at rest when the encrypt option is enabled. 

- HashiCorp Vault secures, stores, and tightly controls access to tokens, passwords, and other sensitive values.
</details>
