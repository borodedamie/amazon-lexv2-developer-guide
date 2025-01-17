# Obscuring slot values in logs<a name="monitoring-obfuscate"></a>

Amazon Lex V2 enables you to obfuscate, or hide, the contents of slots so that the content is not visible\. To protect sensitive data captured as slot values, you can enable slot obfuscation to mask those values for logging\.

When you choose to obfuscate slot values, Amazon Lex V2 replaces the value of the slot with the name of the slot in conversation logs\. For a slot called `full_name`, the value of the slot would be obfuscated as follows:

```
Before:
    My name is John Stiles
After:
    My name is {full_name}
```

If an utterance contains bracket characters \(\{\}\) Amazon Lex V2 escapes the bracket characters with two back slashes \(\\\\\)\. For example, the text `{John Stiles}` is obfuscated as follows:

```
Before:
    My name is {John Stiles}
After:
    My name is \\{{full_name}\\}
```

Slot values are obfuscated in conversation logs\. The slot values are still available in the response from the `RecognizeText` and `RecognizeUtterance` operations, and the slot values are available to your validation and fulfillment Lambda functions\. If you are using slot values in your prompts or responses, those slot values are not obfuscated in conversation logs\.

In the first turn of a conversation, Amazon Lex V2 obfuscates slot values if it recognizes a slot and slot value in the utterance\. If no slot value is recognized, Amazon Lex V2 does not obfuscate the utterance\.

On the second and later turns, Amazon Lex V2 knows the slot to elicit and if the slot value should be obfuscated\. If Amazon Lex V2 recognizes the slot value, the value is obfuscated\. If Amazon Lex V2 does not recognize a value, the entire utterance is obfuscated\. Any slot values in missed utterances won't be obfuscated\.

Amazon Lex V2 also doesn't obfuscate slot values that you store in request or session attributes\. If you are storing slot values that should be obfuscated as an attribute, you must encrypt or otherwise obfuscate the value\.

Amazon Lex V2 doesn't obfuscate the slot value in audio\. It does obfuscate the slot value in the audio transcription\.

All slots are obfuscated by default\. However, you don't need to obfuscate all of the slots in a bot\. You can choose which slots obfuscate using the console or by using the Amazon Lex V2 API\. In the console, choose **Slot obfuscation** in the settings for a slot\. If you are using the API, set the `obfuscationSetting` field of the slot to `DEFAULT_OBFUSCATION` when you call the [CreateSlot](API_CreateSlot.md) or [UpdateSlot](API_UpdateSlot.md) operation\.