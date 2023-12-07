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
https://stt.ins8.ai/api/v1/stt/recognize?api_token={TOKEN}&language=en-sg&timestamp=True&itn=True
```
</br>

## Parameters/Features Supported for Batch Processing (CPU)

Features to specify in request:

```python
`api_token` - Your generated API Token.

`language` - Refer to Language Model. (en-sg, bh-sg, bh-id, ch-sg, ch-ml)

`punctuation` - True/False

`timestamp` - True/False

`confidence` - True/False

`enable_speaker_diarization` - True/False

`diarization_speaker_count` - Int (Between 2 to 5)

`pii` - True/False (Redacts Name, IC, Location, Email and Date)

`itn` - True/False (Inverse normalizes text e.g. there are twenty eight birds -> there are 28 birds)
```

</br>

Features to specify in request body:

```python
`custom_vocab` - 'metaverse,bitcoin' (Enables greater emphasis on key words)

`boost` - Float (Between -20.0 to 20.0)
```
</br>

## Additional parameters/features Supported for Batch Processing (GPU required)

Additional Features supported:

```python
`sentiment` - True/False (Performs sentiment analysis [Postive, Negative, Neutral] and provides justification)

`summary` - True/False (Summarizes audio file in approximately 100 words)
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
--form 'audio=@"{Path_to_audio_file}"' \
--form 'custom_vocab="metaverse,bitcoin"' \
--form 'boost="8.0"'
```

</div>

## Python

<div style="max-height:60vh; overflow-x:scroll ; overflow-y: scroll;">

```python
import requests
path = "Path_to_audio_file.wav"

payload = {'custom_vocab': 'metaverse,bitcoin','boost': '8.0'}

def send_rest_api_request(audio_file, api_token):
    response = requests.post("https://stt.ins8.ai/api/v1/stt/recognize",
                            params= {'api_token': api_token, 'language':'en-sg', 'punctuation':True, 'timestamp':False, 'itn': True},
                            files = {'audio': open(audio_file, 'rb')},
                            data = payload
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

</br>

## Punctuation, ITN and PII removal
<div style="max-height:30vh; overflow-x:scroll ; overflow-y: scroll;">

```python 3
{
    "status": "success",
    "metadata": {
        "sample_rate": 8000,
        "created": "2023-12-06 08:07:08.613346",
        "duration": 28.142,
        "channels": 1,
        "model": "en-sg"
    },
    "channels": [
        {
            "alternatives": {
                "transcript": "Good afternoon. My name is ---- and I am a debt collector with X Y Z company. Your bill payment is past the due date. Based on our records. The total outstanding balance of $10000 is overdue. We have tried contacting you seven times during the last two weeks. Please call us when you get some free time. Also, let us know the status of the payment or if you have any questions. Thank you for your time and attention.",
                "sentences": [
                    {
                        "sentence": "Good afternoon.",
                        "confidence": 0.95527464,
                        "words": [
                            {
                                "word": "good afternoon",
                                "start_time": 1.085,
                                "end_time": 1.644
                            }
                        ]
                    },
                    {
                        "sentence": "My name is ---- and I am a debt collector with X Y Z company.",
                        "confidence": 0.9943339,
                        "words": [
                            {
                                "word": "my",
                                "start_time": 2.281,
                                "end_time": 2.322,
                                "confidence": 0.99828523
                            },
                            {
                                "word": "name",
                                "start_time": 2.403,
                                "end_time": 2.444,
                                "confidence": 0.99996674
                            },
                            {
                                "word": "is",
                                "start_time": 2.567,
                                "end_time": 2.607,
                                "confidence": 0.99994874
                            },
                            {
                                "word": "john",
                                "start_time": 2.77,
                                "end_time": 2.811
                            },
                            {
                                "word": "and",
                                "start_time": 3.301,
                                "end_time": 3.341,
                                "confidence": 0.99980444
                            },
                            {
                                "word": "i",
                                "start_time": 3.545,
                                "end_time": 3.586,
                                "confidence": 0.9999255
                            },
                            {
                                "word": "am",
                                "start_time": 3.79,
                                "end_time": 3.912,
                                "confidence": 0.9771927
                            },
                            {
                                "word": "a",
                                "start_time": 3.994,
                                "end_time": 4.035,
                                "confidence": 0.99901366
                            },
                            {
                                "word": "debt",
                                "start_time": 4.157,
                                "end_time": 4.32,
                                "confidence": 0.9840336
                            },
                            {
                                "word": "collector",
                                "start_time": 4.361,
                                "end_time": 4.687,
                                "confidence": 0.99640405
                            },
                            {
                                "word": "with",
                                "start_time": 4.85,
                                "end_time": 4.891,
                                "confidence": 0.9999651
                            },
                            {
                                "word": "x",
                                "start_time": 4.973,
                                "end_time": 5.054,
                                "confidence": 0.999823
                            },
                            {
                                "word": "y",
                                "start_time": 5.136,
                                "end_time": 5.258,
                                "confidence": 0.92113703
                            },
                            {
                                "word": "z",
                                "start_time": 5.34,
                                "end_time": 5.462,
                                "confidence": 0.9956987
                            },
                            {
                                "word": "company",
                                "start_time": 5.625,
                                "end_time": 5.666,
                                "confidence": 0.99970883
                            }
                        ]
                    },
                    {
                        "sentence": "Your bill payment is past the due date.",
                        "confidence": 0.983567,
                        "words": [
                            {
                                "word": "your",
                                "start_time": 6.922,
                                "end_time": 6.963,
                                "confidence": 0.99961555
                            },
                            {
                                "word": "bill",
                                "start_time": 7.045,
                                "end_time": 7.087,
                                "confidence": 0.99981445
                            },
                            {
                                "word": "payment",
                                "start_time": 7.292,
                                "end_time": 7.498,
                                "confidence": 0.9998294
                            },
                            {
                                "word": "is",
                                "start_time": 7.622,
                                "end_time": 7.663,
                                "confidence": 0.99998
                            },
                            {
                                "word": "past",
                                "start_time": 7.828,
                                "end_time": 7.869,
                                "confidence": 0.8021609
                            },
                            {
                                "word": "the",
                                "start_time": 8.075,
                                "end_time": 8.116,
                                "confidence": 0.9951734
                            },
                            {
                                "word": "due",
                                "start_time": 8.198,
                                "end_time": 8.24,
                                "confidence": 0.99991214
                            },
                            {
                                "word": "date",
                                "start_time": 8.404,
                                "end_time": 8.445,
                                "confidence": 0.99950194
                            }
                        ]
                    },
                    {
                        "sentence": "Based on our records.",
                        "confidence": 0.9912655,
                        "words": [
                            {
                                "word": "based",
                                "start_time": 9.465,
                                "end_time": 9.506,
                                "confidence": 0.99336374
                            },
                            {
                                "word": "on",
                                "start_time": 9.715,
                                "end_time": 9.757,
                                "confidence": 0.99998724
                            },
                            {
                                "word": "our",
                                "start_time": 9.84,
                                "end_time": 9.965,
                                "confidence": 0.9918426
                            },
                            {
                                "word": "records",
                                "start_time": 10.299,
                                "end_time": 10.508,
                                "confidence": 0.917382
                            }
                        ]
                    },
                    {
                        "sentence": "The total outstanding balance of $10000 is overdue.",
                        "confidence": 0.9981337,
                        "words": [
                            {
                                "word": "the",
                                "start_time": 10.941,
                                "end_time": 10.982,
                                "confidence": 0.9973907
                            },
                            {
                                "word": "total",
                                "start_time": 11.105,
                                "end_time": 11.31,
                                "confidence": 0.9983895
                            },
                            {
                                "word": "outstanding",
                                "start_time": 11.433,
                                "end_time": 11.801,
                                "confidence": 0.9970106
                            },
                            {
                                "word": "balance",
                                "start_time": 11.883,
                                "end_time": 12.088,
                                "confidence": 0.9902214
                            },
                            {
                                "word": "of",
                                "start_time": 12.17,
                                "end_time": 12.211,
                                "confidence": 0.9998766
                            },
                            {
                                "word": "$10000",
                                "start_time": 12.375,
                                "end_time": 12.989
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

## Sentiment and Summary 
<div style="max-height:30vh; overflow-x:scroll ; overflow-y: scroll;">

```python 3
{
    "status": "success",
    "metadata": {
        "sample_rate": 16000,
        "created": "2023-12-06 09:24:13.701950",
        "duration": 91.588813,
        "channels": 1,
        "model": "en-sg"
    },
    "channels": [
        {
            "alternatives": {
                "transcript": "the plan increase in goods and services tax will help singapore generate the revenue in needs to invest in its people and social infrastructure six finance minister lawrence wong on wednesday mister wong made the comments in a video on his facebook page about a week before the budget twenty twenty two statement is delivered on february eighteen singapore is at a critical turning point as he continues to deal with a covid nineteen pandemic he said adding that the government is working hard to build a better singapore for tomorrow to do so we will need to invest more in our people and our social infrastructure the g s t increase will help generate the revenues we need for this purpose said mr wong the additional revenue will go towards supporting singapore's growing healthcare needs and enable it to better take care of senior citizens he added the plan to raise the g s t by two percentage points from seven percent to nine percent was first announced in twenty eighteen during then finance minister heng swee keat budget speech mr heng said then that the increase will take place sometime from twenty twenty one to twenty twenty five with the exact timing to be decided based on factors such as the state of the economy it was held off last year due to the impact of covid nineteen on the economy however prime minister this year not indicated in his new year message the government would have to start moving on a plan height in budget twenty twenty two as the economy emerges from covid nineteen",
                "sentences": [
                    {
                        "sentence": "the plan increase in goods and services tax will help singapore generate the revenue in needs to invest in its people and social infrastructure"
                    },
                    {
                        "sentence": "six finance minister lawrence wong on wednesday"
                    },
                    {
                        "sentence": "mister wong made the comments in a video on his facebook page about a week before the budget twenty twenty two statement is delivered on february eighteen"
                    },
                    {
                        "sentence": "singapore is at a critical turning point as he continues to deal with a covid nineteen pandemic"
                    },
                    {
                        "sentence": "he said"
                    },
                    {
                        "sentence": "adding that the government is working hard to build a better singapore for tomorrow"
                    },
                    {
                        "sentence": "to do so"
                    },
                    {
                        "sentence": "we will need to invest more in our people and our social infrastructure"
                    },
                    {
                        "sentence": "the g s t increase will help generate the revenues we need for this purpose said mr wong"
                    },
                    {
                        "sentence": "the additional revenue will go towards supporting singapore's growing healthcare needs and enable it to better take care of senior citizens"
                    },
                    {
                        "sentence": "he added"
                    },
                    {
                        "sentence": "the plan to raise the g s t by two percentage points"
                    },
                    {
                        "sentence": "from seven percent to nine percent"
                    },
                    {
                        "sentence": "was first announced in twenty eighteen during then finance minister"
                    }, ...
                ],
                "sentiment": "Positive",
                "sentiment_justification": "The sentiment of the input is positive. The statement explains how the increase in goods and services tax will help Singapore generate the revenue needed to invest in its people and social infrastructure. The additional revenue will also go towards supporting Singapore's growing healthcare needs and enabling it to better take care of senior citizens. The plan to raise the GST by two percentage points from seven percent to nine percent was first announced in 2018 and has been held off due to the impact of COVID-19 on the economy. However, Prime Minister has indicated that the government would have to start moving on a plan height in budget 2022 as the economy emerges from COVID-19.",
                "summary": "Singapore Finance Minister Lawrence Wong announced that the government plans to increase the goods and services tax (GST) from 7% to 9% in order to generate revenue for investing in people and social infrastructure. The additional revenue will be used to support healthcare needs and improve care for senior citizens. The GST increase was first announced in 2018 and was held off last year due to the impact of COVID-19 on the economy. Prime Minister Lee Hsien Loong has indicated that the government will start moving on this plan in Budget 2022 as the economy emerges from COVID-19."
            }
        }
    ]
}
```
</div>

