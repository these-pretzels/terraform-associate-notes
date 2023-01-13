## Objective 1: Understand Infrastructure as Code (IaC) concepts

<details><summary>Explain What IaC is?</summary>
<p>
Allow you to manage infra using config files. You can build, change, and manage infra in safe and consistent way. You do this by defining resource configs.

HashiCorp Terraform is an infrastructure as code tool that lets you define both cloud and on-prem resources in human-readable configuration files that you can version, reuse, and share. You can then use a consistent workflow to provision and manage all of your infrastructure throughout its lifecycle. Terraform can manage low-level components like compute, storage, and networking resources, as well as high-level components like DNS entries and SaaS features.
</details>

<details><summary>Describe Day 0 and Day 1?</summary>
 <p>
- **Can be applied throughout the infrastructure lifecycle, both on the initial and life of the infra. Day 0 and Day1**
  - IAC in private or public cloud
  - Day 0 : Initial Build 
  - Day 1 : OS and application config you apply after the initial build. Includes OS updates, patches, app config. 
  - TF include libraries of providers and modules that make it easy to write and provision infra. If we need to apply Day 1 configs, then code can use chef/ansible etc. 
</details>
<details><summary>Describe advantages of IaC patterns?</summary>
<p>
  - **Saves time by making it easy to provision and apply infrastructure configuration.** Workflow is **standardized** across providers wether its VMWare, AWS, Azure, or GCP. 
- **It's easy to understand** the intent of infrastructure changes. 
- **Iac makes changes idempotent**:
  - The result will always be the same since the same code is being applied 
- **Iac makes changes consistent**:
  -  The manual work is removed with Iac no more need for system administrators to remotely connect to each machine by executing a series of commands or scripts which can cause inconsistencies based on who executes it
- **Iac makes changes predictable**:
  - code can be tested before applying it to production so results are always predictable 
- **Iac allows for mutation in previously defined configurations, making for a more manageable system**

</details>

## Objective 1: Understand Infrastructure as Code (IaC) concepts
