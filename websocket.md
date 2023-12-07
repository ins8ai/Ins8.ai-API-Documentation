# Supported Languages and Audio Formats
The WebSocket Endpoints supports all languages as listed on [here](/getting_started.md#supported-languages-and-features) and the following audio formats

| **File Format**    | **Audio Bit Depth** | **Max File Size**                  |**Languages**                             |
|--------------------|---------------------|------------------------------------|------------------------------------------|
| RAW                | PCM 16-bit          | 10MB                               |Singapore/Malaysia English, Bahasa Melayu |


You can try with this [Sample audio](https://stt.ins8.ai/samples/sample.wav) !

</br>
</br>

# Parameters/Features Supported for Websocket Streaming
Whenever an API request is made to Ins8.ai STT, API Token must be included as a HTTP Parameter.

To enable a specific feature e.g. Punctuation, Timestamp, the corresponding must be added as a HTTP Parameter with the value True in the URL. 

```python
wss://stt.ins8.ai/api/v1/stt/websocket/recognize?api_token={TOKEN}&timestamp=True
```

## Parameters/Features Supported for Websocket Stream

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
</br>

# Opening Websocket Connection

When creating a WebSocket Connection, the base url should look like

```python
wss://stt.ins8.ai/api/v1/stt/websocket/recognize?api_token={TOKEN}&language=en-sg
```

</br>
</br>

# Initiating Recognition Request

After a connection is initialized, a client will have to first send a configuration packet in json detailling the configuration and metadata details of the audio to be sent.

## Parameters
``` python
`encoding` - Specify the type of encoding the audio file. Please use `raw`.

`sample_rate_hertz` - Sample rate in kilohertz e.g. 16000.

`stop_string` - The stop string that marks the end of an audio transmission.

`custom_voab` - Enable greater emphasis on key words

`boost` - Float value between -20.0 to 20.0
```

</br>

After sending the initial start message, the client can begin transmitting audio packets to the websocket in the format as defined during the initial transmission.

## Example of packet

```python
{'encoding': 'raw', 'sample_rate_hertz': 16000, 'stop_string': 'EOS', 'custom_vocab':'metaverse,bitcoin', 'boost':8}
```

<br />



</br>
</br>

# End of Recognition

From the websocket server, the websocket client can close the websocket connection. Upon receiving:

```python
!!END_OF_TRANSCRIPTION!!
```

## Python
<div style="max-height:30vh; overflow-x:scroll ; overflow-y: scroll;">

```python 3
# Install required packages using
# pip install pywav
# pip install websockets

import json
from pywav import WavRead
import asyncio
import ssl
import websockets

path = "Path_to_example_audio.wav"


async def receiver(websocket):
    """ (Asynchronous) function to recieve transcript for chunks asynchronously

    Args:
        websocket ([websockets.legacy.client.WebSocketClientProtocol])
        
    """
    
    while True:
        data = await websocket.recv()
        print(f"Hypothesis of recognised speech: {data}")
        if data == "!!END_OF_TRANSCRIPTION!!":
            break

async def sender(websocket, data: list, stop_str:str ):
    """ 
        (Asynchronous) function to send audio chunks asynchronously
    Args:
        websocket ([websockets.legacy.client.WebSocketClientProtocol])
        data (list): [ List of audio chunks in bytes ]
        stop_str (str): [ the string that is sent to the server to mark the end of audio streaming]
        
    """
    
    for chunk in data:
        await asyncio.sleep(0.05)
        await websocket.send(chunk)
    # ! Required: A stop string has to be sent
    #await websocket.send(stop_str)
    await websocket.send(stop_str.encode())

def get_chunks(filepath, filetype=None):
    if not filetype:
        filetype = filepath.split('.')[-1]
    # Loads the audio into memory
    assert filetype.lower() == 'wav', f"WSS endpoint only support wave file."
    audio = WavRead(filepath)
    sample_rate = audio.getsamplerate()
    assert (audio.getnumofchannels() == 1)
    content = audio.getdata()
    chunk_size = int(sample_rate * audio.getbytespersample() / 10) # 100ms
    assert (audio.getaudioformat() in [1, 7])
    assert audio.getaudioformat() == 1, "WSS endpoint only support PCM encoded wave files"
    encoding = 'raw' 
        
    config = {'encoding': encoding, 
              'sample_rate_hertz': sample_rate, 
            #   stop string could string of any choice
              'stop_string': 'EOS'}   
    stream = [content[i:i+chunk_size] for i in range(0, len(content), chunk_size)]
    return config, stream


async def wss_test(hostname="wss://stt.ins8.ai/api/v1/stt/websocket/recognize", token=''):
    audio_config, data_stream_list = get_chunks(filepath=path)
    url = hostname + "?api_token=" + token + "&language=en-sg" + "&itn=True" + "&punctuation=True" + "&pii=True"
    ssl_context = None
    ssl_context = ssl.SSLContext(ssl.PROTOCOL_TLS_CLIENT)
    ssl_context.check_hostname = False
    ssl_context.verify_mode = ssl.CERT_NONE
    if ssl_context is not None:
        async with websockets.connect(url, ssl=ssl_context) as websocket:
            #, ssl=True
            stop_string = audio_config['stop_string']
            await websocket.send(json.dumps(audio_config))
            print(audio_config)
            while True:
                await asyncio.gather(sender(websocket, data_stream_list, 
                                            stop_string
                                            ), 
                                receiver(websocket))
                break
            print(f"Client closing connection")
    else:
        async with websockets.connect(url) as websocket:
            #, ssl=True
            stop_string = audio_config['stop_string']
            await websocket.send(json.dumps(audio_config))
            print(audio_config)
            while True:
                await asyncio.gather(sender(websocket, data_stream_list, 
                                            stop_string
                                            ), 
                                receiver(websocket))
                break
            print(f"Client closing connection")


api_key = "<YOUR SERVICE TOKEN>"
asyncio.get_event_loop().run_until_complete(wss_test(token=api_key))
```

</div>

# Sample JSON output

Below is the sample JSON output obtained from Ins8.ai STT Websocket streaming.

## Default (Timestamp disabled)

```python
Testing testing, one, two three.
```

## Timestamp enabled
<div style="max-height:50vh; overflow-x:scroll ; overflow-y: scroll;">

```python
{
   "transcript":"Testing testing, one, two three.",
   "confidence":0.982926,
   "words":[
      {
         "startTime":"3.424s",
         "endTime":"3.668s",
         "word":"testing",
         "confidence":0.92237765,
         "start_time":3.424,
         "end_time":3.668
      },
      {
         "startTime":"3.871s",
         "endTime":"4.114s",
         "word":"testing",
         "confidence":0.919446,
         "start_time":3.871,
         "end_time":4.114
      },
      {
         "startTime":"4.357s",
         "endTime":"4.398s",
         "word":"one",
         "confidence":0.99807906,
         "start_time":4.357,
         "end_time":4.398
      },
      {
         "startTime":"4.560s",
         "endTime":"4.601s",
         "word":"two",
         "confidence":0.9857209,
         "start_time":4.56,
         "end_time":4.601
      },
      {
         "startTime":"4.763s",
         "endTime":"4.804s",
         "word":"three",
         "confidence":0.9959221,
         "start_time":4.763,
         "end_time":4.804
      }
   ]
}
```
</div>