# SmartCardReader

The SmartCardReader library facilitates the interaction with the smartcard and the smartcard reader.

The library currently supports the following smartcards:

* BEID Card


## Getting started (physical connected cardreader)
### Get all the smartcard readers
```
//Gets the names of all the connected readers for the device
var readers = ISmartCardReader.GetReaders();
```

### Setup the connection
```
//create a logger from Microsoft.Extensions.Logging library
var loggerFactory = LoggerFactory.Create(
                builder =>
                {
                    builder.AddConfiguration(config.GetSection("Logging"))
                   // builder.SetMinimumLevel(LogLevel.Debug)
                    //builder.AddFilter("", LogLevel.Debug)
                    .AddConsole();
                });

var logger = loggerFactory.CreateLogger<Program>();

//Get the smartcard reader you want to interact with and pass the name to SmartCardReader class (ex readers[0])
SmartCardReader _smartCardReader = new SmartCardReader(logger, readers[0]);

_smartCardReader.Connect();

_smartCardReader.CardInserted += smartCardReader_CardInserted;
_smartCardReader.CardRemoved += smartCardReader_CardRemoved;

```

### Process the events and read the card of the data

```
void smartCardReader_CardRemoved(object? sender, EventArgs e)
{
    Console.WriteLine("The card was removed.");
}

void smartCardReader_CardInserted(object? sender, EventArgs e)
{
    var card = (BEIDCard)_smartCardReader.GetStatus().SmartCard;
    
    var idInfo = card.GetId();
    var addressInfo = card.GetAddress();
    var photo = card.GetPhoto();
    var atr = card.GetAtr();
    
    //do stuff
}

```

## Getting started (Virtual cardreader)

If there is no cardreader to connect there is also the possibility to drop a json file in the submap "/Import" 
The file name that will be watched should be passed to the VirtualSmartCardReader(logger, filename)
The card inserted event will be triggered when the file is dropped.
The card removed event will be triggered when the file is deleted.



###  Setup the connection

```
var loggerFactory = LoggerFactory.Create(
                builder =>
                {
                    builder.AddConfiguration(config.GetSection("Logging"))
                   // builder.SetMinimumLevel(LogLevel.Debug)
                    //builder.AddFilter("", LogLevel.Debug)
                    .AddConsole();
                });
VirtualSmartCardReader _smartCardReader = new VirtualSmartCardReader(_logger, "filename");

_smartCardReader.Connect();

_smartCardReader.CardInserted += smartCardReader_CardInserted;
_smartCardReader.CardRemoved += smartCardReader_CardRemoved;

```
### Process the events and read the card of the data

```
void smartCardReader_CardRemoved(object? sender, EventArgs e)
{
    Console.WriteLine("The card was removed.");
}

void smartCardReader_CardInserted(object? sender, EventArgs e)
{
    var card = (BEIDCard)_smartCardReader.GetStatus().SmartCard;
    
    var idInfo = card.GetId();
    var addressInfo = card.GetAddress();
    var photo = card.GetPhoto();
    var atr = card.GetAtr();
    
    //do stuff
}

```
