# Playbook

This repository contains the source code for a website whichs hosts example scenarios.

## Dependencies

- Hugo
- Asciidoctor
- Text to speech from Google Cloud

## Development

### Compile ADOC to HTML

```
cd lib/2021-12-11_register-patient
asciidoctor index.adoc
```

### Generate MP3

```
export GOOGLE_APPLICATION_CREDENTIALS=$(pwd)/mykey.json

INDEX=99
TEXT='Example.'
SLUG='my-example-speeach

read -r -d '' PAYLOAD << EOM
{
  "input":{
    "text":"$TEXT"
  },
  "voice":{
    "languageCode":"en-gb",
    "name":"en-GB-Standard-B",
    "ssmlGender":"MALE"
  },
  "audioConfig":{
    "audioEncoding":"MP3"
  }
}
EOM

  
echo "" | curl -X POST \
  -H "Authorization: Bearer "$(gcloud auth application-default print-access-token) \
  -H "Content-Type: application/json; charset=utf-8" \
  -d "$PAYLOAD" \
  https://texttospeech.googleapis.com/v1/text:synthesize | jq -r '.audioContent' | base64 -d > lib/${INDEX}_${SLUG}.mp3
```
