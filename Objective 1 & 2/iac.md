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
  - IAC in private or public cloud.nTF include libraries of providers and modules that make it easy to write and provision infra. If we need to apply Day 1 configs, then code can use chef/ansible etc. 
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
	Write: Define infra resources in config files
	Plan: Review changes TF will make to your infra 
	Apply: TF provisions your infra in the correct order and updates state file
</details>

## Objective 2: Understand Terraform's purpose (vs other IaC)
