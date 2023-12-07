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
| Singapore/Malaysia English   | en-sg          | Punctuation, Timestamp, [Inverse Text Normalization](#supported-languages-and-features  "Inverse text normalization (ITN) is a process that converts spoken text into written text. ITN example: I have one hundred and twenty-three dollars -> I have $123."), [Pii redaction](#supported-languages-and-features "Personal Identificable Information (PII) redaction is a process that masks out sensitive information from transcribed audio transcript. It is able to mask our Name, NRIC, Email, Location and Date. PII example: this is mary speaking -> this is ---- speaking"), [Speaker Diarization](#supported-languages-and-features "Speaker diarization is a process that identifies and segments an audio recording by the number of unique speakers detected") [Custom Vocab](#supported-languages-and-features "Custom vocab supports the emphasis of user-specified keywords so as to improve the audio transcription process and receive a more accurate transcript")|
| Bahasa Melayu   | bh-sg          | Default Transcription  |


</br>
</br>

## HTTP Parameters for features

When making an API Request, you can enable or disable specific features by specifying them as HTTP Parameters in the URL.


| **CPU Features**               | **HTTP Parameter Example** | **Default**  |
|---------------------------|----------------------------|--------------|
| API Token Authentication  | ?api_token={api_token}     | Input API Token         |
| Punctuation               | ?punctuation=True          | False        |
| Timestamp                 | ?timestamp=True            | False        |
| Confidence(Word + Sentence)                | ?confidence=True           | False        |
| Speaker Diarization                | ?enable_speaker_diarization=True?diarization_speaker_count=2           | False        |
| Pii redaction             | ?pii=True                  | False        |
| Inverse text normalization| ?itn=True                  | False        |

<br/>

| **GPU Features**               | **HTTP Parameter Example** | **Default**  |
|---------------------------|----------------------------|--------------|
| Summarization  | ?summary=True    | False        |
| Sentiment Analysis  | ?sentiment=True    | False        |

<br/>

## Request body for features

When making an API Request, you can enable or disable specific features by specifying them as HTTP Parameters in the URL.


| **CPU Features**               | **Form Body Example** | **Default**  |
|---------------------------|----------------------------|--------------|
| Custom Vocab | custom_vocab='metaverse,bitcoin'       | None    |
| Boost (Increasing/decreasing the emphasis of the custom vocab) | boost=8.0  | boost=8.0 (Between float value -20.0 to 20.0)       |

<br/>

Multiple features can be enabled or disabled at once by using & between different parameters, for example:
</br>
<div style={{ maxHeight:"50vh", width:"70vw"}}>


Transcribing audio with punctuation, pii and itn activated:

```python
https://stt.ins8.ai/api/v1/stt/recognize?api_token=<TOKEN>punctuation=True&pii=True&itn=True&language=<MODEL-NAME>
```
</div>