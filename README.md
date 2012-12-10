# Web MIDI API Polyfill

This JS library is a prototype polyfill and shim for the [Web MIDI API](https://dvcs.w3.org/hg/audio/raw-file/tip/midi/specification.html) (of which I am a co-author), using [Jazz-Soft.net's Jazz-Plugin](http://jazz-soft.net/) to enable MIDI support on Windows and OSX.  This polyfill and the plugin should work on Chrome, Firefox, Safari, Opera and IE.

I'm currently using this polyfill to test usability of the API itself, but it's also useful to enable MIDI scenarios.

##There are a couple of unimplemented things currently:

1. I don't yet correctly fire onmessage as an event dispatch; it's just a function call.
2. Sending long MIDI messages like sysex - that is, any message longer than 3 bytes - isn't yet supported.  Likewise, timestamps on send are currently ignored, although it is next on my list to fix (although the timing will not be very precise).
3. Jazz doesn't expose the version number or manufacturer, so these are always "<not supported>".

##Usage

1. Copy the WebMIDIAPI.js file from /lib/ into your project.  
2. Add "&lt;script src='lib/WebMIDIAPI.js'>&lt;/script>" to your code.

Now you can use the Web MIDI API as captured in the specification (except for the exceptions noted above) - it will automatically check to see if the Web MIDI API is already implemented, and if not it will insert itself.

So, some sample usage: 

	var m = null;   // m = MIDIAccess object for you to make calls on
    navigator.getMIDIAccess( onsuccesscallback, onerrorcallback );
    function onsuccesscallback( access ) { 
    	m = access;

    	// Things you can do with the MIDIAccess object:
	    var inputs = m.enumerateInputs();   // inputs = array of MIDIPorts
	    var outputs = m.enumerateOutputs(); // outputs = array of MIDIPorts
	    var i = m.getInput( inputs[0] );    // grab first input device.  You can also getInput( index );
	    i.onmessage = myMIDIMessagehandler;	// onmessage( event ), event.MIDIMessages = []MIDIMessage.
	    var o = m.getOutput( outputs[0] );  // grab first output device
	    o.sendMessage( [ 0x90, 0x45, 0xff ] );  // full velocity note on A4 on channel zero
	    o.sendMessage( [ 0x80, 0x45, 0x00 ] );  // A4 note off
	};

Better documentation later.  :)
