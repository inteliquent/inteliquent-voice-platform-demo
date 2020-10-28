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

