# Voice Platform Script Demo
This repository contains information on how to attach a custom call flow Voice API script to a Voyant Phone Number.

# The Script
This script ([in this repository](https://github.com/inteliquent/voyant-voice-platform-demo/blob/master/script-example.xml)) accomplishes the following use-case:
- We receive the inbound Call (LegA) and:
  - Play ringing in the background
  - Then, we Dial out to the SIP Endpoint for legB
    - When the user picks up, it's either the voicemail, or a real person, so we ask them to press a Key to accept the call (using TTS)
      - If they press 1, then we join them to LegA so the two parties can talk
      - If they don’t press 1, then we terminate LegB and transition LegA to Voicemail:
        - Stop playing ringback on LegA
        - Play the voicemail instructions using TTS
        - Play a Beep
          - Turn on call recording to record a voicemail.
  - Once the call is over, or other significant events occur, we send those events via web hook to a backend for recording.
        

# Event Logs

The script will POST events to the following HTTP Request Logger using our `query` XML verb:

http://webhook.site/#!/7dc48a5d-f66e-464f-a208-3411c139fda4


# Assigning the Script to a Phone Number
POST https://api.voyant.com/voice/api/{{PHONE_NUMBER}}/script?sessionId={{API_TOKEN}}

BODY: {{SCRIPT XML CONTENT}}

## CURL Example

```
curl -X POST \
  'https://api.voyant.com/voice/api/17372213556/script?sessionId={{API_TOKEN}}' \
  -H 'Accept: */*' \
  -H 'Accept-Encoding: gzip, deflate' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Content-Length: 7204' \
  -H 'Content-Type: application/xml' \
  -H 'Host: api.voyant.com' \
  -d '<script>
    <head></head>
    <body>        
        <!-- Accept the inbound call -->
        <accept></accept>

        <!-- Send back 180 ringing -->
        <ring></ring>

        <!-- Answer the call -->
        <answer alias="{{X-OrigCallId}}">
            <!-- When the call connects, Say "Hello World" -->
            <onConnect>
                <say TEXT="Hello World"></say>
            </onConnect>
        </answer>
    </body>
</script>'
```

# Test Phone Numbers

The XML script in this repository is currently associated with 1 phone number:
- 1 (737) 221-3556

The script associated with this number can be edited in the Voyant Admin Portal or via our REST API.

**Voyant Admin Portal Example**

![Edit via Admin Portal](https://www.dropbox.com/s/tsqzdjs1ymqten1/Screenshot%202019-12-11%2013.00.16.png?dl=0&raw=true)

**REST API Example**

GET https://api.voyant.com/voice/api/{{PHONE_NUMBER}}/script?sessionId={{API_TOKEN}}

## CURL Example

```
curl -X GET \
  'https://api.voyant.com/voice/api/17372213556/script?sessionId={{API_TOKEN}}' \
  -H 'Accept: */*' \
  -H 'Accept-Encoding: gzip, deflate' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Content-Type: application/xml' \
  -H 'Host: api.voyant.com' \
  -H 'User-Agent: PostmanRuntime/7.18.0' \
```

# Testing with a SIP Client

The script is configured to dial out to a registered SIP User Agent. To test this functionality, download the "Cloud SoftPhone" App in the [Apple App Store](https://apps.apple.com/us/app/cloud-softphone/id567475545) or [Google Play Store](https://play.google.com/store/apps/details?id=cz.acrobits.softphone.cloudphone&hl=en_US).

The app will register via SIP and will ring if one of the test numbers (associated with the script in this repository) are called.

After opening the app for the first time, scan one of these QR codes to provision the application for this demo.

*Test User 1*

![Test User 1 Provisioning QR Code](https://www.dropbox.com/s/nyjb1u5e2cwtsj3/ACR1.png?dl=0&raw=true)

*Test User 2*

![Test User 2 Provisioning QR Code](https://www.dropbox.com/s/1i28d03l6fvyhd8/ACR2.png?dl=0&raw=true)

# Further Documentation

Documentation on assigning and unassigning scripts to phone numbers can be found here:

https://voyant.docs.apiary.io/#reference/voice-api/add-voice-api-service-to-a-number

Documentation on Voice API XML functionality can be found here:

https://voyantvoiceapi.docs.apiary.io/#introduction/overview


