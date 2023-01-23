## Objective 1: Understand Infrastructure as Code (IaC) concepts

<details><summary>Explain What IaC is?</summary>
<p>
Allow you to manage infra using config files. You can build, change, and manage infra in safe and consistent way. You do this by defining resource configs.

HashiCorp Terraform is an infrastructure as code tool that lets you define both cloud and on-prem resources in human-readable configuration files that you can version, reuse, and share. You can then use a consistent workflow to provision and manage all of your infrastructure throughout its lifecycle. Terraform can manage low-level components like compute, storage, and networking resources, as well as high-level components like DNS entries and SaaS features.
</details>

<details><summary>Describe Day 0 and Day 1?</summary>
 <p>
  - Can be applied throughout the infrastructure lifecycle, both on the initial and life of the infra. Day 0 and Day1 
<p>
  - IAC in private or public cloud. TF include libraries of providers and modules that make it easy to write and provision infra. If we need to apply Day 1 configs, then code can use chef/ansible etc. <br>
  - Day 0 : Initial Build 
  - Day 1 : OS and application config you apply after the initial build. Includes OS updates, patches, app config. 
</details>

<details><summary>Describe advantages of IaC patterns?</summary>
<p>

- (Manage) TF can manage infra on multiple cloud platforms. Plugins called providers let TF interact with other services using API. 

- (Automate) TF is human readable and declarative to help standardize workflow. TF config describe the desired end-state you want.

- (Track) TF state file allow you to track resource changes throughout deployment and acts as source of truth. TF uses state file to determine changes made to your infra. 

- (Collaborate) TF can commit configs to version control to safely contribute on infra. TFC is used to securely share state file with your team. TFC can connect to VCS (version control system) allowing you to auto propose when you commit.

- (Standardize) TF support reusable config component called modules, that define configurable collection of infra. Iac makes changes idempotent. The result will always be the same since the same code is being applied 
</details>

<details><summary> IAC infra more reliable </summary>
<p>

We can test and review the code before it’s applied to target environments. Once ready to use, we apply that code via automation. Code is checked into version control system, we can review how infra evolve over time. 
Idempotent from IAC ensures result will be the same, no matter if same code applied multiple times.
</details>

<details><summary> IAC infra more manageable </summary>
<p>
During execute, TF will examine current state of infra. Determine differences between current state and revised desired state. Indicates the necessary changes that need to be applied. When approved, only necessary changes will be applied. 
</details>

<details><summary>How does TF work </summary>
<p>
TF creates and manages resources on cloud and other platforms through application programming interfaces (API). 
Core TF workflow:
	- Write: Define infra resources in config files
	- Plan: Review changes TF will make to your infra 
	- Apply: TF provisions your infra in the correct order and updates state file
</details>

## Objective 2: Understand Terraform's purpose (vs other IaC)
<details><summary>Explain multi-cloud and provider-agnostic benefits</summary>
<p>
TF can use same workflow to manage multiple providers and handle cross-cloud dependencies. Simplifies management and orchestration for large scale infras. This means in the event of failure there is a more graceful recovery of a region or provider. 
<p>
TF use cases <p>
Application deployment, scaling and monitoring tools <br>
Use TF to deploy, release, scale, monitor multi-tier applications. TF allows you to manage resources in each tier together and auto handle dependencies between tiers.

Self service cluster <br>
Use TF to build self service infra model that lets teams manage their own infra independently. TF modules can be used to codify standards for deploying and managing services. 

Policy compliance and management  <br>
Use sentinel to automatically enforce compliance and governance policies before TF make infra changes. Sentinel available with TFC team and governance tier. 

PaaS application setup <br>
Some platform vendors allow you to create web apps and attach add-ons. TF can codify the setup required for these PaaS setups.

Software defined networking <br>
TF can interact with SDNs to auto configure network according to the app requirements. 
Kubernetes <br>

Open source workload scheduler for containerized applications. TF can both deploy and manage its resources (pods, deployments, services). 

Parallel environments <br>
TF allows you to rapidly spin up and decomm infra for dev / test / QA / prod. Can create disposable environments as needed, also cost efficient. 

Software demos <br>
TF to create, provision, bootstrap demo on different cloud providers. Allow user to try software on own infra.
</details>

<details><summary>Explain the benefits of state	</summary>
<p>
State is necessary for TF to function. Purpose of state file. <p>

**Mapping to real world**<br>
TF requires database to map config to real world. IE: a resource like `aws_instance` maps to an actual ec2 instance `i-123abcd`
Cannot use tags because note all resources support tags, and not all cloud providers support tags. TF uses its own state structure to map resources to real world.

**Metadata** <br>
TF must also track metadata, like resource dependencies. When we delete resource from config, TF must know how to delete from remote system. 
To ensure correct operation, TF retains copy of most recent set of dependencies within state.

**Performance** <br>
TF store cache of attribute values for all resources in state. Done only as a performance improvement.
When running `terraform plan`, TF must know current state of resources, in order to determine the changes it needs to make - per the desired config.
- Small infras, TF can query providers and sync latest attributes from all resources. This is default for TF: every plan and apply.
- Large infras, TF querying every resource is too slow. Users can use `-refresh=false` flag and `-target` flag to work around this. In this case, the cached state is treated as source of truth.

**Syncing**  <br>
Default config, TF stores state in a file in current working directory. 
As team grows, *Remote state* is the recommended solution. TF can use remote locking as a measure to avoid multiple users from running TF at the same time. Also to make sure that each TF run begins with the most recent updated state.

</details>
