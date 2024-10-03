# Troubleshooting Voicemail Express
Below are some common problems customers have encountered with the appropriate resolution.

## General troubleshooting tips
-  If you are experiencing issues, set the logging level of the Lambda functions to DEBUG. You can change this setting by going to the 4 core functions (VMXKVStoS3, VMXTranscriber, VMXPackager, VMXPresigner) and changing the Environment Variable for **lambda_logging_level** to `DEBUG`. This will provide the most detail.
-  The flow of events, at a high level, is:
    -  CTR is emitted
    -  VMXKVStoS3 function
    -  VMXTranscriber function
    -  VMXPackager function
    -  VMXPresigner function (VMXPacakger calls this function)
-  Begin troubleshooting at the VMXKVStoS3 function. Check the CloudWatch logs to see if the function succeeded and that the file was created and placed in S3
-  Once you validate that the file was created, look to see if there were any errors in the VMXTranscriber function. Depending on how closely you are monitoring progression, you can also look in the Transcribe console to see if the Transcription process is still running, completed, or returned an error.

## General Issues
### The voicemail audio is garbled when I play it back
Voicemail Express uses the Amazon Kinesis Video Stream to capture the recording. It is only expecting there to be one audio stream, from the customer, when it begins recording. If you have selected both **From the customer** and **To the customer** when initializing your KVS stream in your contact flow, the VMXKVStoS3 function will attempt to mix the two channels together, causing garbled audio. Make sure that you are only enabling the **From the customer** option when starting streaming.

### I am not getting any voicemails
Make sure that you are setting the **vmx_flag** value to `1` in your contact flows. This must be set in order for the VMXKVStoS3 function to know that it should process this record as a voicemail.

## Removal Issues
### The VMXContactFlowStack fails to delete, causing the parent stack to fail as well.
Make sure that you remove the test flow from any phone numbers in your Amazon Connect instance. If it is associated to a phone number, the delete will fail. Once you make that change, re-try the delete and it should now succeed.

### The VMXCoreStack fails to delete, causing the parent stack to fail as well.
If there are any files in the S3 buckets created by the stack, the delete will fail intentionally. You must decide what to do with your files. You can opt to delete them, move them, or re-run the delete, choosing the option to keep the bucket resources. If you delete them or move them, re-run the delete and it should succeed.