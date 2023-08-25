# Supported Languages and Audio Formats
The REST Endpoints supports all languages as listed [here](/getting_started.md#supported-languages-and-features) and the following audio formats

| **File Format**    | **Audio Encoding & Type** | **Max File Size**   |**Languages**                              |
|--------------------|---------------------|-----------------------|-------------------------------------------|
| .wav (uncompressed)              | PCM 16-bit, Single and Multi-channel| 10MB | Singapore/Malaysia English, Bahasa Melayu |

</br>



Try with this [Sample Audio](https://stt.ins8.ai/samples/sample.wav) !

</br>
</br>

# Authentication
Whenever an API request is made to Ins8.ai STT, API Token must be included as a HTTP Parameter.

To enable a specific feature e.g. Punctuation, Timestamp, the corresponding must be added as a HTTP Parameter with the value True in the URL. 

```python
https://stt.ins8.ai/api/v1/stt/recognize?api_token={TOKEN}&timestamp=True
```
</br>

## Parameters/Features Supported for Batch Processing

```python
`api_token` - Your generated API Token.

`language` - Refer to Language Model.

`punctuation` - True/False

`timestamp` - True/False

`confidence` - True/False

`enable_speaker_diarization` - True/False

`diarization_speaker_count` - Int (Between 2 to 5)
```

</br>
</br>

# Batch Processing

Ins8.ai STT supports <b>Single</b> and <b>Multi-channel audio file</b> for batch processing(Audio file upload).
The transcription output will be based on the number of channels detected in the audio file.

Try it with the <b>cURL</b> command or sample <b>Python script</b> provided, simply copy and change the required parameter!

## cURL
<div style="max-height:30vh; overflow-x:scroll ; overflow-y: scroll;">

```python
curl --location 'https://stt.ins8.ai/api/v1/stt/recognize?api_token=<API_TOKEN>&punctuation=True&language=en-sg&timestamp=False' \
--form 'audio=@"{Path_to_audio_file}"'
```

</div>

## Python

<div style="max-height:60vh; overflow-x:scroll ; overflow-y: scroll;">

```python
import requests
path = "Path_to_audio_file.wav"

def send_rest_api_request(audio_file, api_token):
    response = requests.post("https://stt.ins8.ai/api/v1/stt/recognize",
                            params= {'api_token': api_token, 'language':'en-sg', 'punctuation':True, 'timestamp':False},
                            files = {'audio': open(audio_file, 'rb')}
                            )
    return response

api_key = "<API_TOKEN>"
response = send_rest_api_request(path, api_key)

## To view the response from the terminal
print(response.json())
```

</div>

<span>
  Replace <strong>API_TOKEN</strong> with your generated API Token.
  <br/>
  Replace <strong>"Path to audio file"</strong> with the directory where your audio file is located.
</span>

<br />
<br />

# Sample JSON output

Below are the sample JSON output obtained from Ins8.ai STT Batch Processing API Request.

## Single channel
<div style="max-height:30vh; overflow-x:scroll ; overflow-y: scroll;">

```python 3
{
    "status": "success",
    "metadata": {
        "sample_rate": 44100,
        "created": "2023-07-13 08:53:56.155432",
        "duration": 76.307188,
        "channels": 1,
        "model": "en-sg"
    },
    "channels": [
        {
            "alternatives": {
                "transcript": "Thank you for calling. How may I assist you today? Hi, I am S. W. May I know when the next iphone will be released? The next iphone model will be released sometime this October. I see how can I upgrade my mobile plan from youth combo three to silver combo Six. You can update your mobile plan when your plan is up for recontract in June. Which is two months from now? Would you want us to reach out to you? When it is time for you to recontract. Yes, please, can you whatsapp me then? Yes. I'll put a note in the system. For us to do that. Is there anything else I can help you with? Oh, there's one more thing. I have a line that's no longer has a contract with the other telco. The service is so bad. I hope to move it. Over to you can. Of course. We would love to be able to serve you better. Can I whatsapp you the steps to port your number over? That would be great. Is there anything else I can help you with? No, you have been awesome, Siy, thank you so much. Thank you for calling us. Glad to be your service, goodbye. Bye.",
                "sentences": [
                    {
                        "sentence": "Thank you for calling. How may I assist you today?",
                        "confidence": 0.987933,
                        "words": [
                            {
                                "word": "thank",
                                "start_time": 1.582,
                                "end_time": 1.706,
                                "confidence": 0.90890944
                            },
                            {
                                "word": "you",
                                "start_time": 1.706,
                                "end_time": 1.747,
                                "confidence": 0.9998908
                            }, ...
                            {
                                "word": "today",
                                "start_time": 3.565,
                                "end_time": 3.606,
                                "confidence": 0.9999529
                            }
                        ]
                    },
                    {
                        "sentence": "Hi, I am S. W. May I know when the next iphone will be released?",
                        "confidence": 0.95483726,
                        "words": [
                            {
                                "word": "hi",
                                "start_time": 4.662,
                                "end_time": 4.702,
                                "confidence": 0.9998944
                            },
                            {
                                "word": "i",
                                "start_time": 5.028,
                                "end_time": 5.069,
                                "confidence": 0.9992199
                            },
                            {
                                "word": "am",
                                "start_time": 5.15,
                                "end_time": 5.232,
                                "confidence": 0.6102775
                            }, ...
                        ]
                    }, ...
                ]
            }
        }
    ]
}
```
</div>

</br>

## Multi-channel
<div style="max-height:30vh; overflow-x:scroll ; overflow-y: scroll;">

```python 3
{
    "status": "success",
    "metadata": {
        "sample_rate": 8000,
        "created": "2023-07-13 08:58:06.220733",
        "duration": 2.936625,
        "channels": 2,
        "model": "en-sg"
    },
    "channels": [
        {
            "alternatives": {
                "transcript": "Hi my name is Mary.",
                "sentences": [
                    {
                        "sentence": "Hi my name is Mary.",
                        "confidence": 0.8700776,
                        "words": [
                            {
                                "word": "hi",
                                "start_time": 0.286,
                                "end_time": 0.327,
                                "confidence": 0.90170884
                            },
                            {
                                "word": "my",
                                "start_time": 0.409,
                                "end_time": 0.736,
                                "confidence": 0.9106034
                            }, ...
                            {
                                "word": "mary",
                                "start_time": 1.555,
                                "end_time": 1.595,
                                "confidence": 0.97395384
                            }
                        ]
                    }
                ]
            }
        },
        {
            "alternatives": {
                "transcript": "Hi this is John speaking.",
                "sentences": [
                    {
                        "sentence": "Hi this is John speaking.",
                        "confidence": 0.95990336,
                        "words": [
                            {
                                "word": "hi",
                                "start_time": 0.246,
                                "end_time": 0.287,
                                "confidence": 0.9617587
                            },
                            {
                                "word": "this",
                                "start_time": 0.575,
                                "end_time": 0.616,
                                "confidence": 0.78538203
                            }, ...
                            {
                                "word": "speaking",
                                "start_time": 1.686,
                                "end_time": 1.727,
                                "confidence": 0.9992943
                            }
                        ]
                    }
                ]
            }
        }
    ]
}
```
</div>

</br>

## Diarization
<div style="max-height:30vh; overflow-x:scroll ; overflow-y: scroll;">

```python 3
{
    "status": "success",
    "metadata": {
        "sample_rate": 44100,
        "created": "2023-07-13 09:00:46.550406",
        "duration": 76.307188,
        "channels": 1,
        "model": "en-sg"
    },
    "channels": [
        {
            "alternatives": {
                "transcript": "Thank you for calling. How may I assist you today? Hi, I am S. W. May I know when the next iphone will be released? ....",
                "sentences": [
                    {
                        "sentence": "Thank you for calling. How may I assist you today?",
                        "confidence": 0.987933,
                        "words": [
                            {
                                "word": "thank",
                                "start_time": 1.582,
                                "end_time": 1.706,
                                "confidence": 0.90890944,
                                "speaker_tag": 1
                            },
                            {
                                "word": "you",
                                "start_time": 1.706,
                                "end_time": 1.747,
                                "confidence": 0.9998908,
                                "speaker_tag": 1
                            },
                            {
                                "word": "for",
                                "start_time": 1.83,
                                "end_time": 1.871,
                                "confidence": 0.9999739,
                                "speaker_tag": 1
                            }, ...
                        ]
                    },
                    {
                        "sentence": "Hi, I am S. W. May I know when the next iphone will be released?",
                        "confidence": 0.95483726,
                        "words": [
                            {
                                "word": "hi",
                                "start_time": 4.662,
                                "end_time": 4.702,
                                "confidence": 0.9998944,
                                "speaker_tag": 2
                            },
                            {
                                "word": "i",
                                "start_time": 5.028,
                                "end_time": 5.069,
                                "confidence": 0.9992199,
                                "speaker_tag": 2
                            },
                                ...
                            {
                                "word": "may",
                                "start_time": 6.209,
                                "end_time": 6.25,
                                "confidence": 0.9997484,
                                "speaker_tag": 2
                            }, ...
                        ]
                    }, ...
                ]
            }
        }
    ]
}
```
</div>


