# androidspeech

This is an Android library containing an API to Mozilla's speech recognition services. 


## Installation
```
dependencies { 
      implementation 'com.github.mozilla:mozillaspeechlibrary:1.0.6'
}
```

## Demo app
Just run the demo application inside the folder `app`

## Usage
The API encapsulates the microphone capture, audio encoding, voice activity detection and network 
communication. So for this reason its integration is very simple: just call `start()` and handle the events in your frontend.

#### First define a listener to capture the events:
```
ISpeechRecognitionListener mVoiceSearchListener = new ISpeechRecognitionListener() {
      public void onSpeechStatusChanged(final MozillaSpeechService.SpeechState aState, final Object aPayload){
          runOnUiThread(new Runnable() {
              @Override
              public void run() {
                  switch (aState) {
                      case DECODING:
                          // Handle when the speech object changes to decoding state
                          break;
                      case MIC_ACTIVITY:
                          // Captures the activity from the microphone
                          double db = (double)aPayload * -1;
                          break;
                      case STT_RESULT:
                          // When the api finished processing and returned a hypothesis 
                          string transcription = ((STTResult)aPayload).mTranscription;
                          float confidence = ((STTResult)aPayload).mConfidence;
                          break;
                      case START_LISTEN:
                          // Handle when the api successfully opened the microphone and started listening
                          break;
                      case NO_VOICE:
                          // Handle when the api didn't detect any voice
                          break;
                      case CANCELED:
                          // Handle when a cancelation was fully executed
                          break;
                      case ERROR:
                          // Handle when any error occurred
                          string error = aPayload;
                          break;
                      default:
                          break;
                  }
              }
          });
      }
  };
```

#### Then start it:
```
        mMozillaSpeechService = MozillaSpeechService.getInstance();
        mMozillaSpeechService.setLanguage("en-US");
        mMozillaSpeechService.addListener(mVoiceSearchListener);
        mMozillaSpeechService.start(getApplicationContext());
```

#### In the case you want to cancel a progressing operation:
```
        mMozillaSpeechService.cancel();
```

#### To remove a listener:
```
        mMozillaSpeechService.removeListener(aListener);
```

**Note**: Your app will need `RECORD_AUDIO`, `WRITE_EXTERNAL_STORAGE` and `READ_EXTERNAL_STORAGE` permissions to be [set](https://github.com/mozilla/androidspeech/blob/master/app/src/main/AndroidManifest.xml#L5) in AndroidManifest.xml manifest and [requested](https://github.com/benfrancis/androidspeech/blob/master/app/src/main/java/com/mozilla/speechapp/MainActivity.java#L78) at runtime.
