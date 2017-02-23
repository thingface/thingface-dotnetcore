# The thingface client library for .NET Core
simple client library as nuget package

## Installation

```sh
Install-Package ThingfaceClientNetCore
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
            thingface.OnCommand(CommandHandler);
            thingface.SendSensorValue("temp", 12.3);
        }
    }

    public static void Main(string[] args)
    {
        var thingface = new ThingfaceClient("mydevice", "mydevicesecret", "my-app.thingface.io");
        thingface.ConnectionStateChanged += ConnectionStateChanged;
        thingface.Connect();

        Console.Read();

        thingface.Disconnect();
    }
}

```

## API Reference
API is very simple. Have a look to api reference.

### new ThingfaceClient(string deviceId, string deviceSecretKey, string host)
thingface client constructor
- `deviceId` - device ID
- `deviceSecretKey` - secret key for that device
- `host` - device gateway hostname

### thingface.Connect()
connect to the thingface device gateway specified by the given host name with current device ID and device secret key.

### thingface.Disconnect()
disconnect from thingface device gateway

### thingface.IsConnected()
returns *true* if this client is connected, otherwise it returns *false*. Use it everytime when you need to check if device is connected or not.

### event thingface.ConnectionStateChanged
connection state event handling

### thingface.OnCommand(Action<CommandContext> commandHandler = null, string sender = null);
subscribe for commands from sender
- `commandHandler` (optional) - a function to handle commands
- `sender` (optional) - sender id (username or device ID), if sender is not provided device will receive commands from every user or device

### thingface.OffCommand(string sender = null);
unsubscribe for commands from sender
- `sender` (optional) - sender id (username or device ID)

### thingface.SendSensorValue(string sensorId, double sensorValue)
send sensor value to thingface gateway
- `sensorId` - sensor ID from the device
- `sensorValue` - current sensor value

## More Information
- [https://github.com/thingface](https://github.com/thingface)
- [http://thingface.io](http://thingface.io/)