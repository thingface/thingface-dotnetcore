# The thingface client library for .NET
simple client library as nuget package for .NET Core, .NET Framework 4.6.2 and Micro Framework 4.3

## Installation

```sh
Install-Package Thingface.Client
```

## Code Example

A few lines of code and you're ready to control or monitor your device.

```csharp
public class Program
{
    private static void CommandHandler(CommandContext context){
        if (context.CommandName == "say")
        {
            Console.WriteLine(context.CommandArgs[0]);
        }
    }

    private static void ConnectionStateChanged(object sender, ConnectionStateEventArgs eventArgs)
    {
        if (eventArgs.NewState == ConnectionState.Connected)
        {
            var thingface = (IThingfaceClient)sender;
            thingface.OnCommand(CommandHandler); // subscribe to commands from all (users and devices)
            thingface.SendSensorValue("temp", 12.3);
        }
    }

    public static void Main(string[] args)
    {
        var thingface = new ThingfaceClient("mydeviceid", "mydevicesecretkey");
        thingface.ConnectionStateChanged += ConnectionStateChanged;
        thingface.Connect();

        Console.Read();

        thingface.Disconnect();
    }
}

```

## API Reference
API is very simple. Have a look to api reference.

### new ThingfaceClient(string deviceId, string deviceSecretKey, string host = "personal.thingface.io", int port = 8883, bool enableSsl = true)
thingface client constructor
- `deviceId` - device ID
- `deviceSecretKey` - secret key for that device
- `host` (optional) - device gateway hostname (default **personal.thingface.io**)
- `port` (optional) - device gateway port (default **8883** for SSL connection, **1883** for no SSL connection)
- `enableSsl` (optional) - enable SSL connection (default **true**)

### thingface.Connect()
connect to the thingface device gateway specified by the given host name with current device ID and device secret key.

### thingface.Disconnect()
disconnect from thingface device gateway

### thingface.IsConnected()
returns *true* if this client is connected, otherwise it returns *false*. Use it everytime when you need to check if device is connected or not.

### event thingface.ConnectionStateChanged
connection state event handling

### event thingface.CommandReceived
command received event handling

### thingface.OnCommand(Action<CommandContext> commandHandler = null, SenderType senderType = SenderType.All, string senderId = null);
subscribe for commands from sender
- `commandHandler` (optional) - a function to handle commands
- `senderType`(optional) - sender type User or Device
- `senderId` (optional) - sender ID (username or device ID), if sender is not provided device will receive commands from every user or device

### thingface.OffCommand(SenderType senderType = SenderType.All, string senderId = null);
unsubscribe for commands from sender
- `senderType`(optional) - sender type User or Device
- `senderId` (optional) - sender ID (username or device ID), if sender is not provided device will receive commands from every user or device

### thingface.SendSensorValue(string sensorId, object sensorValue)
Send sensor value to thingface server. The value can be only `double` or `long` type.
- `sensorId` - sensor ID from the device
- `sensorValue` - current sensor value

## More Information
- [https://github.com/thingface](https://github.com/thingface)
- [https://thingface.io](https://thingface.io/)
