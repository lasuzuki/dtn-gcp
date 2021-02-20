# Testing your connection 

Assuming no errors occur with the configuration above, we are now ready to test loopback communications. In one terminal, we have to run the start script (the one we said that you would have to have earlier). It's right here, in case you forgot to write it down:

````
ionstart -i host1.ionrc -l host1.ltprc -b host1.bprc -p host1.ipnrc
````

This command will run the appropriate administration programs, in order, with the appropriate configuration files. Don't worry that the command is lengthy and unwieldly; we will show you how to make a more clean single configuration file later.

Once the daemon is started, run:

````
bpsink ipn:1.1 &
````

This will begin constantly listening on the Endpoint ID with the endpoint_number 1 on service_number 1, which is used for testing.

Now open another terminal and run the command:

````
bpsource ipn:1.1
```` 

This will begin sending messages you type to the Endpoint ID ipn:1.1, which is currently being listened to by bpsink. Type messages into bpsource, press enter, and see if they are reported by bpsink.


# Stopping the Daemon

As the daemon launches many ducts and helper applications, it can be complicated to turn it all off. To help this, we provided a script. The script similar to ionstart exists called `ionstop`, which tears down the ion node in one step. You can call it like so:

````
# shell script to stop node
#!/bin/bash
tccadmin 203    .
sleep 1
tcaadmin 203    .
sleep 1
bpadmin         .
sleep 1
ltpadmin        .
sleep 1
ionadmin        .
````

After stopping the daemon, it can be restarted using the same procedures as outlined above. Do remember that the ion.log file is still present, and will just keep growing as you experiment with ION.


