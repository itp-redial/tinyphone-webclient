###Tinyphone web client
Tinyphone apps can be built in Javascript using the Tinyphone web client.  The web client makes it possible to handle phone call events inside a browser.

The easiest way to install the web client is to clone the project from Github.

    git clone git://github.com/itp-redial/tinyphone-webclient
The Github repo contains the client, called “tinyphone_client.js“, and some example web pages.

The tinyphone client is expecting to connect to the [tinyphone server](https://github.com/itp-redial/tinyphone).  Make sure that you have installed Asterisk and the Tinyphone Server.

One of the examples, “tinyphone_simple.html“, is posted below. Tinyphone_simple.html is a good template for your projects. As you can see, there’s 3 basic steps to configuring a tinyphone app.

 - Include tinyphone_client.js in your html page.
 - Initialize tinyphone client with the tinyphone server with server location, port, and phone number that people will call.
 - register listeners for the 4 tinyphone events: new_call, keypress, audio_level, and hangup.  All listeners are optional.  For example, if you don’t care about audio level events, you can leave out that listener.

````
    <html>
        <body>
            <script type="text/javascript" src="tinyphone_client.js"></script>
            <script>
                var phoneNumber = "1 (360) 555-1212";
                var cr = "<br/>"; //add a line break to each addition to page.
                tinyphone.init("yourhost.com",12003,phoneNumber);
                tinyphone.on('connect', function(){
                    document.write("connected to tinyphone with phone number "+tinyphone.phoneNumber+cr);
                });
                tinyphone.on('new_call', function(caller){
                    document.write("new caller "+caller.callerNumber+" (label "+caller.callerLabel+"), id "+caller.id+", args ["+caller.args.join(", ")+"]"+cr);
                });
                tinyphone.on('keypress', function(caller){
                    document.write("received keypress '"+caller.keypress+"' from "+caller.callerNumber+cr);
                });
                tinyphone.on('audio_level', function(caller){
                    document.write("received audio level '"+caller.audioLevel+"' from "+caller.callerNumber);
                });
                tinyphone.on('hangup', function(caller);
                    document.write("caller "+caller.callerNumber+"( id "+caller.id+") hung up"+cr);
                });
            </script>
        </body>
    </html>
````

All other examples included with tinyphone_client.js are expansions on the simple template.

####Tinyphone Web Client Events
You can register for any or all of these events by using **tinyphone.on(...)**.  Use tinyphone_simple.html and tinyphone_simple_sms.html as a guide.
 - **new_call**
  - A new caller object has been created and stored in tinyphone_client, and is passed to app.
  - id: Every call has unique ID.
  - callerNumber: The caller's phone number (callerID).
  - callerLabel: An obscured version of the caller's phone number, suitable for public display.
  - args: an array of any arguments that were passed from the Tinyphone server.
 - **keypress**
  - keypress: updated keypress in caller object, 0-9, *, or #
 - **audio_level**
  - audioLevel: updated audio level in caller object, range from 0.0 - 1.0
 - **hangup**
  - caller object has been removed from tinyphone_client, and is passed to app.
 - **connect**
  - tinyphone_client has successfully connected to the tinyphone server
 - **disconnect** 
  - tinyphone_client has disconnected from the tinyphone server
 - **sms** 
  - tinyphone_client has received an SMS message
  - id: Every sms has unique ID.
  - callerNumber: The caller's phone number (callerID).
  - callerLabel: An obscured version of the caller's phone number, suitable for public display.
  - message: The SMS message body.
