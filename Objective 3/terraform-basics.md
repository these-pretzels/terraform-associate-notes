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