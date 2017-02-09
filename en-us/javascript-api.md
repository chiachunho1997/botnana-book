# Javascript API

Botnana Control API can be downloaded at [npm registry](https://www.npmjs.com/).

While using nodejs, add the following dependency in your `package.json`.

    botnana: '*'

Then import the package into your program:

    var botnana = require("botnana");

`botnana` package can also be used in a browser.

## Examples

Following example uses Websocket to connect to Botnana Control, 
then obtains the version of Botnana Control:

    var botnana = require("botnana");

    botnana.on("version", function(version) {
        console.log("version: " + version);
    })

    botnana.once("ready", function() {
        botnana.version.get();
    });

    // Start communicating with Botnana Control at 'ws://192.168.7.2:3012'.
    // If the programm is running on a Botnana Control board, IP address should
    // be 'ws://localhost:3012'.
    botnana.start('ws://192.168.7.2:3012');

## Start、Ready and Poll

The program connects to Botnana Control at `ip_address` using `botnana.start(ip_address)`. 
Event `ready` means you have connected to Botnana Control and can start processing request.
`botnana.start` will automatically start polling Botnana Control every 100 ms.

    botnana.once("ready", function() {
        // work 1 //工作 1
        // work 2
        // ...
    });

    botnana.start("ws://192.168.7.2:3012");

## Data Event API

Botnana Control's response format:

    tag1|value1|tag2|value2...

After processing by `botnana.handle_response(response)`, tags convert to events.
Data Event API can be used to process these events. e.g.

    botnana.on("version", function(version) {
        console.log("version: " + version);
    })
    botnana.on("log", function (log) {
        console.log("log: " + log);
    });
    botnana.on("error", function (err) {
        console.log("err: " + err);
    });
    botnana.on("homing_method.3", function (value) {
        console.log("Homing method of slave 3 is " + result);
    });
    botnana.on("dout.2.1", function (value) {
        console.log("dout 1 of slave 2 is " + value);
    });

To only process the event once, use `once`. e.g.

    botnana.once("dout.2.1", function (value) {
        console.log("dout 1 of slave 2 is " + value);
    });

Or use `times` to specify the number of times to process the event. e.g.

    botnana.times("dout.2.1", function (value) {
        console.log("dout 1 of slave 2 is " + value);
    }, 5);

## Version API

e.g.

    botnana.on("version", function (version) {
        console.log("version: " + version);        
    });
    botnana.once("ready", function() {
        botnana.version.get();
    })

## Configuration API

Configuration API can process the configuration file.

### Altering the configuration

Edits done to the configuration will not be immediately saved, 
and will not affect devices in use.

e.g. Altering slave 1's homing method within the configuration file. 

    botnana.config.set_slave({
      position: 1,
      tag: "homing_method",
      value: 33
    });

Edits done to the configuration file will not be immediately saved, 
and will not affect current EtherCAT configuration.

### Saving configuration

Saving configuration will immediately alter the configuration file, 
but will not affect parameters in use by devices.

Reboot will apply new configurations. 

e.g. Save configurations: 

    botnana.config.save();

## Slave API

### Reading slave info

`get()` can obtain slave info. e.g.

        botnana.ethercat.slave(1).get();

After response data is processed by function `botnana.handle_response(response)`, 
a corrensponding event will be emitted. 
Event API can be used to process these events.

e.g. Obtaining homing method of slave at position 1.

    botnana.on("homing_method.1", function (homing_method) {
        console.log("result: " + homing_method);
    });
    botnana.once("ready", function() {
        botnana.ethercat.slave(1).get();
    });

### Configuring drive

Command to configure motor drive:

    botnana.ethercat.slave(i).set(tag, value);

e.g. Configuring drive's homing method: 

    botnana.ethercat.slave(1).set("homgin_method", 33);

### Clearing drive error

    botnana.ethercat.slave(i).reset_fault();

### Writing and reading IO

e.g. Digital and analogue output/input：

    botnana.on("dout.1.5", function (value) {
        console.log("dout 5 of slave 1 is " + value);
    });
    botnana.on("din.2.4", function (value) {
        console.log("din 4 of slave 5 is " + value);
    });
    botnana.on("aout.3.2", function (value) {
        console.log("aout 2 of slave 3 is " + value);
    });
    botnana.on("ain.4.2", function (value) {
        console.log("ain 2 of slave 2 is " + value );
    });
    botnana.once("ready", function() {
        botnana.ethercat.slave(1).set_dout{1, 1);
        botnana.ethercat.slave(3).set_aout(1, 30);
        botnana.ethercat.slave(1).get();
        botnana.ethercat.slave(2).get();
        botnana.ethercat.slave(3).get();
        botnana.ethercat.slave(3).get();
    });

e.g. Some slave's Analog IO is required to be enabled.

    botnana.ethercat.slave(1).disable_aout(5);
    botnana.ethercat.slave(1).enable_aout(5);
    botnana.ethercat.slave(1).disable_ain(2);
    botnana.ethercat.slave(1).enable_ain(2);

## Real-time Programming API

A simple real-time program:

    var p1 = new botnana.Program("p1");
    p1.deploy();
    // When finished deploying, execute the program.
    botnana.once("deployed", function() {
        p1.run();
    })

* `deploy()`: Deploy program to real-time thread.
  Event `deployed` will be emitted when deployment completed.
* `run()`: Execute deployed program.

Clear all deployed programs：

    botnana.empty();

e.g. A program that will home the drive, then move the drive to position 30000:

    var p2 = new botnana.Program("p2");
    var s1 = p2.ethercat.slave(1);
    s1.hm();
    s1.move_to(30000);
    p2.deploy();
    botnana.once("deployed", function() {
        p2.run();
    });

e.g. A program that will first move two drives home then move them to position (30000,40000).

    var p3 = new botnana.Program("p3");
    var s1 = p3.ethercat.slave(1);
    var s2 = p3.ethercat.slave(2);
    s1.hm();
    s2.hm();
    s1.go();
    s2.go();
    s1.pp();
    s2.pp();
    s1.move_to(30000);
    s2.move_to(40000);
    s1.go();
    s2.go();
    p3.deploy();
    botnana.once("deployed", function() {
        p3.run();
    });

The following program utilizes `until_target_reached()` 
forcing the second axes to move after the first:

    var p4 = new botnana.Program("p4");
    var s1 = p3.ethercat.slave(1);
    var s2 = p3.ethercat.slave(2);
    s1.hm();
    s2.hm();
    s1.go();
    s1.until_target_reached();
    s2.go();
    s1.pp();
    s2.pp();
    s1.move_to(30000);
    s1.go();
    s1.until_target_reached();
    s2.move_to(40000);
    s2.go();
    p4.deploy();
    botnana.once("deployed", function() {
        p4.run();
    });
