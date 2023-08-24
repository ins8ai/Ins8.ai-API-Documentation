# Creating an account

To access Ins8.ai’s STT Demo, sign up for a trial account on our website https://dev.ins8.ai

All trial accounts will be provided with 15 minutes of total transcription time across all services, models and features.

![image](https://stt.ins8.ai/samples/register.png)

</br>
</br>

# Obtaining an API Token

After creating an account, you can obtain an API Token and start using our STT API.

![image](https://stt.ins8.ai/samples/token.png)

</br>
</br>

# Supported Languages and Features

Ins8.ai officially supports the following languages: Singapore/Malaysia English and Bahasa Melayu. We are currently working on other Southeast Asian language models in the pipeline. 

Interested to learn more about what we have to offer? Contact us at contact@ins8.ai

| **Language model** | **Model name** | **Features available**                              |
|--------------------|----------------|-----------------------------------------------------|
| Singapore/Malaysia English   | en-sg          | Punctuation, Timestamp, [Inverse Text Normalization](#supported-languages-and-features  "Inverse text normalization (ITN) is a process that converts text that has been spoken into text that is written in a more standard format. ITN example: If someone says I have one hundred and twenty-three dollars, Ins8.ai would convert that into I have $123.") |
| Bahasa Melayu   | bh-sg          | Default Transcription  |


</br>
</br>

## HTTP Parameters for features

When making an API Request, you can enable or disable specific features by specifying them as HTTP Parameters in the URL.


| **Feature**               | **HTTP Parameter Example** | **Default**  |
|---------------------------|----------------------------|--------------|
| API Token Authentication  | ?api_token={api_token}     | Input API Token         |
| Punctuation               | ?punctuation=True          | False        |
| Timestamp                 | ?timestamp=True            | False        |
| Confidence(Word + Sentence)                | ?confidence=True           | False        |
| Speaker Diarization                | ?enable_speaker_diarization=True?diarization_speaker_count=2           | False        |

<br/>

Multiple features can be enabled or disabled at once by using & between different parameters, for example:
</br>
<div style={{ maxHeight:"50vh", width:"70vw"}}>

```python
https://stt.ins8.ai/api/v1/stt/recognize?api_token=<TOKEN>punctuation=True&timestamp=True&language=<MODEL-NAME>
```
</div>