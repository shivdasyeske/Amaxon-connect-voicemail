# Voicemail Express V3
Voicemail Express is designed to provide basic voicemail functionality to Amazon Connect. It has been designed to work seamlessly behind the scenes, providing voicemail options for all agents and queues by default. It is an evolution of the Voicemail Express solution that was created specifically for Service Cloud Voice customers by the AWS team that worked with Salesforce to develop Service Cloud Voice. That solution has been cloned and included with Service Cloud Voice, and is now used at scale by customers on that offering. This version removes the Salesforce-centric options, providing the same easy-to-deploy-and-use voicemail option for standard Amazon Connect customers. Typically speaking, Voicemail Express can be deployed and validated in less than 15 minutes. 

![Voicemail Express Architecture](Docs/Img/VMX3.png)

## What's new in VMX3 (2024.03.20)
-  Simplified deployment process.
-  Removed Salesforce-centric deployment options.
-  All voicemails are delivered as Amazon Connect tasks. The option to add other deliver modes will come in a future release. 
-  An Amazon Connect flow module named **VMX3VoicemailCoreModule** is provided. This provides a standard voicemail experience, sets all required attributes, and records the voicemail. You can use this module in any standard Amazon Connect inbound contact flow to provide the voicemail experience without needing to create a custom flow.
-  The VMX3TestFlow has been modified to use the **VMX3VoicemailCoreModule**.
-  Modified transcribe job name to eliminate conflicts.

### How it works
With Voicemail Express, customers can have the option to leave a voicemail for an agent or queue. Once the voicemail is recorded, a series of processes take place in the following order:
1. Voicemail stored in S3 as a .wav file
2. Transcription of the voicemail
3. Presigned URL that provides access to the voicemail without the need for authentication into the AWS account hosting Amazon Connect.
4. Voicemail is packaged for delivery, including the transcription, presigned URL, and contact data. It is then delivered as an Amazon Connect Task.

Voicemails are configured for a retention period of up to 7 days. After 7 days, the recordings are the presigned URL is no longer valid, and the recordings are lifecycled. During deployment, you have the option to configre the lifecycle window, if desired. Additionally, you have the option to keep, archive, or delete voicemail recordings. 

### How to deploy
To deploy Voicemail Express, you will need to complete the following:
1. Complete the [Voicemail Express Prerequisites](Docs/vmx_prerequistes.md)
1. Complete the [Voicemail Express Installation](Docs/vmx_installation_instructions.md)

### About Voicemail Express
Once Voicemail Express has been deployed, you can learn more about it by reading the [High-level overview of the Voicemail Express solution](Docs/vmx_core.md).

### How to uninstall
To remove Voicemail Express follow the instructions below:
1.  [Removing/Uninstalling Voicemail Express](Docs/vmx_uninstall.md)

Finally, some basic troubleshooting steps can be found on the [Troubleshooting Common Voicemail Issues](Docs/vmx_troubleshooting.md) page.

## Roadmap
The following items are planned for future releases
-  Optional delivery mode add-ins: Allows you to add additional delivery modes as desired. The first batch of delivery modes will be:
   -  Salesforce Case
   -  Salesforce custom objects
   -  Email via SES
-  Update KVStoS3 function to Python
-  Update python version to 3.12
-  Reduce layer size for default deployments
-  Reduce complexity and number of functions
-  Agent queue check to make sure there are not too many contacts in queue.

**Current Published Version:** 2024.03.20
Current published version is the version of the code and templates that has been deployed to our S3 buckets
