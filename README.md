# README

Starting from the good work made before, i just extend in codec8ex some AVL ( ioElements ) and event, and i also add a debug flag tp trace the parser trace the parser used) 

Parsing teltonika binary data from device FMB0XX.
Right now supported Codec8, Codec8 Extended and Codec7 format, Codec 16 yet in process.

### Source

Forked from: https://www.npmjs.com/package/teltonika-parser

### Installation

Run console command

`npm i teltonika-parser-ex`

### Usage example

```const net = require('net');
 const Parser = require('teltonika-parser-ex');
 const binutils = require('binutils64');


 let server = net.createServer((c) => {
     console.log("client connected");
     c.on('end', () => {
         console.log("client disconnected");
     });

     c.on('data', (data) => {

         let buffer = data;
         let parser = new Parser(buffer);
         if(parser.isImei){
             c.write(Buffer.alloc(1, 1));
         }else {
             let avl = parser.getAvl();

             let writer = new binutils.BinaryWriter();
             writer.WriteInt32(avl.number_of_data);

             let response = writer.ByteBuffer;
             c.write(response);
         }
     });
 });

 server.listen(5000, () => {
     console.log("Server started");
 });
```
