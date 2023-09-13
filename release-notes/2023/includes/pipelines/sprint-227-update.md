---
author: ckanyika
ms.author: ckanyika
ms.date: 9/13/2023
ms.topic: include
---


###  Workload Identity Federation in Azure Pipelines (public preview)

Do you want to stop storing secrets and certificates in Azure service connections? Want to stop worrying about rotating these secrets whenever they expire? We are now announcing a public preview of Workload Identity Federation for Azure service connections. [Workload identity federation](https://learn.microsoft.com/azure/active-directory/workload-identities/workload-identity-federation&preserve-view=true) uses an industry-standard technology, Open ID Connect (OIDC), to simplify the authentication between Azure Pipelines and Azure. Instead of secrets, a federation subject is used to facilitate this authentication.

As part of this feature, the Azure (ARM) service connection has been updated with an additional scheme to support Workload identity federation. This allows Pipeline tasks that use the Azure service connection to authenticate using a federation subject (`sc://<org>/<project>/<service connection name>`). The main benefits of using this scheme over existing authentication schemes are as follows:

- Simplified management: You do not need to generate, copy, and store secrets from service principals in AAD to Azure DevOps anymore. Secrets that are used in other authentication schemes of Azure service connections (e.g., service principal) expire after a certain period (2 years currently). When they expire, pipelines fail. You have to regenerate a new secret and update the service connection. Switching to workload identity federation eliminates the need to manage these secrets and improves the overall experience of creating and managing service connections.  
- Improved security: With workload identity federation, there is no persistent secret involved in the communication between Azure Pipelines and Azure. As a result, tasks running in pipeline jobs cannot leak or exfiltrate secrets that have access to your production environments. This has often been a concern for our customers.

You can take advantage of these features in two ways:

- Use the [new workload identity federation scheme](https://aka.ms/azdo-rm-workload-identity) whenever you create a new Azure service connection. Moving forward, this will be the recommended mechanism.
- [Convert](https://aka.ms/azdo-rm-workload-identity-conversion) your existing Azure service connections (which are based on secrets) to the new scheme. You can perform this conversion one connection at a time. Best of all, you do not have to modify any of the pipelines that use those service connections. They will automatically leverage the new scheme once you complete the conversion.

To create a new Azure service connection using workload identity federation, simply select Workload identity federation (automatic) or ([manual](https://aka.ms/azdo-rm-workload-identity-manual)) in the Azure service connection creation experience:

> [!div class="mx-imgBorder"]
> ![ Screenshot of resource manager.](../../media/227-pipelines-05.png " Screenshot of resource manager")

> [!div class="mx-imgBorder"]
> ![ Screenshot of identify federation.](../../media/227-pipelines-06.png " Screenshot of identify federation")

To convert a previously created Azure service connection, select the "Convert" action after selecting the connection:

> [!div class="mx-imgBorder"]
> ![ Screenshot of convert.](../../media/227-pipelines-07.png " Screenshot of convert")

All of the Azure tasks that are included with Azure Pipelines now support this new scheme. However, if you are using a task from the Marketplace or a home-grown custom task to deploy to Azure, then it may not support workload identity federation yet. In these cases, we ask that you update your task to support workload identity federation to improve security. A complete list of supported tasks can be found [here](https://aka.ms/azdo-rm-workload-identity-troubleshooting).

For this preview, we support workload identity federation only for Azure service connections. This scheme does not work with any other types of service connections. See our docs for more details.

###  Pipeline agents can be registered using a Service Principal

As Azure DevOps support for Service Principals is in preview, we have added the option to use a Service Principal to register a Pipelines agent with Azure DevOps Services:
```
--auth 'SP' --clientid 12345678-1234-1234-abcd-1234567890ab --clientsecret --tenantid 12345678-1234-1234-abcd-1234567890ab
```
You can grant the Service Principal access on the security settings of an agent pool. This removes the need to use a Personal Access Token (PAT)

###  Use Service Principal in Agent VM extension

Azure VMs can be included in Deployment Groups using a [VM Extension](/azure/devops/pipelines/release/deployment-groups/howto-provision-deployment-group-agents?view=azure-devops#install-the-azure-pipelines-agent-azure-vm-extension-using-an-arm-template&preserve-view=true). The VM extension has been updated to use a Service Principal instead of a PAT to register the agent:
```
"settings": {
  "userServicePrincipal": true     
}
"protectedSettings": {
  "clientId": "[parameters('clientId')]"      
  "clientSecret": "[parameters('clientSecret')]"      
  "tenantId": "[parameters('tenantId')]"      
}
```
### Azure Active Directory device code authentication flow for pipelines agent registration

The pipelines agent gained support for [Azure Entra ID Device Code Flow](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Device-Code-Flow). You can now use a web browser to easily complete setup instead of manually creating a PAT just for one-time setup. When you run the agent configuration script, enter "AAD" for authentication type. The script will guide you through the next steps, including where to go on the web and what code to enter. After you enter your code on the web, return to the console to finish setting up the agent.

> [!div class="mx-imgBorder"]
> ![ Screenshot of authentication flow.](../../media/227-pipelines-04.png " Screenshot of authentication flow")


### REST APIs for Environments

An [Environment](/azure/devops/pipelines/process/environments?view=azure-devops&preserve-view=true) is a collection of resources that you can target with deployments from a pipeline. Environments provide you deployment history, traceability for work items and commits, and access control mechanisms.

We know you want to create environments [programmatically](https://developercommunity.visualstudio.com/t/rest-api-to-manage-environments-yaml-pipelines/859033), so we published documentation for their [REST API](/rest/api/azure/devops/environments/environments/add).

### Prevent unintended pipeline runs

Today, if your YAML pipeline doesn't specify a `trigger` section, it runs for any changes pushed to its repository. This can create confusion as to why a pipeline ran and lead to many unintended runs.

We added an organization- and project-level Pipelines setting named _Disable implied YAML CI trigger_ that lets you change this behavior. You can choose to not trigger pipelines if their trigger section is missing.

> [!div class="mx-imgBorder"]
> ![ Screenshot of YAML CI trigger.](../../media/227-pipelines-01.png " Screenshot of YAML CI trigger")

### Build GitHub repositories securely by default

Last sprint, we introduced a [centralized control for building PRs from forked GitHub repos](/azure/devops/release-notes/2023/sprint-226-update#centralized-control-for-building-prs-from-forked-github-repos).

With this sprint, we are enabling the `Securely build pull requests from forked repositories` option at the organization level, for new organizations. Existing organizations are unaffected.

### Disabled override of code coverage policy status to Failed when build is failing

Previously in, the code coverage policy status was overridden to 'Failed' if your build in PR was failing. This was a blocker for some of you who had the build as an optional check and the code coverage policy as a required check for PRs resulting in PRs being blocked.

> [!div class="mx-imgBorder"] 
> ![ Screenshot of PRs blocked.](../../media/227-pipelines-02.png " Screenshot of PRs blocked")

With this sprint, the code coverage policy won't be overridden to 'Failed' if the build fails.
This feature will be enabled for all customers.

> [!div class="mx-imgBorder"]
> ![ Screenshot of Results after change.](../../media/227-pipelines-03.png " Screenshot of Results after change")


