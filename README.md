## freeeeg32.js

This tool lets you handle FreeEEG32 data streams in the browser via USB.

```
npm i freeeeg32js
```

```
let eeg = new eeg32(
    onDecodedCallback(newLinesInt),
    onConnectedCallback,
    onDisconnectedCallback,
    CustomDecoder,
    baudrate,
    mode
); //create an eeg streaming device, can specify baudrate, there is also a 250sps mode for testing another version.

// customize the onDecodedCallback, which returns a count for the number of new lines

eeg.getLatestData(channel,count) 

// get the latest chunk of data for a selected channel, use this with the 
// onDecodedCallback to continuously pull the new data,
// which does not show up in uniform chunks to prevent issues around periodic browser 
// slowdowns in high demand applications.

eeg.setupSerialAsync(baudrate); 
//this runs the USB connect command, it will pop up a browser menu.

eeg.closePort(); //disconnect


```

The decoder runs at a throttled rate which you can change in the code as this.readRate. 

This keeps it from eating up browser time as the device streams at 512sps and will lock everything up otherwise. Generally, setting this to a minimum framerate is best for actual applications (e.g. 60fps).

The decoder uses something called a boyerMoore search so as data comes in and builds the buffer up, the search will identify each line of code by special identifier bytes which the decoder then chunks through to build the dataset. 

Timestamps assume zero dropped samples and are incremented linearly, no issues so far there so sample pacing is kept perfect and timed by when the first line of data comes in.


By: Joshua Brewster
License: AGPL v3.0