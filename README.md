<h1 align="center">SignalR</h1>
<p align="center">Understand and implementation of SignalR using .Net Core</p>

Description
--
SignalR is a library for .NET that allows you to add real-time functionality to your web applications. It is designed to enable bi-directional communication between a server and one or more clients. With SignalR, you can easily add features like chat, real-time dashboards, and collaboration tools to your .NET application.

To use SignalR in a .NET Core application, you will need to install the SignalR package from NuGet. To do this, you can use the following command in the Package Manager Console:

Install-Package Microsoft.AspNetCore.SignalR

Once the package is installed, you can add SignalR to your application by following these steps:

 1. In your Startup class, add the following line to the ConfigureServices method to add the SignalR service:
   
   <pre>services.AddSignalR();</pre>
   
2. In the Configure method, add the following line to map the SignalR hubs:

  <pre>app.UseEndpoints(endpoints =>
  {
      endpoints.MapHub<MyHub>("/myHub");
  });</pre>

3. Create a hub class by deriving from Microsoft.AspNetCore.SignalR.Hub. This class will contain the methods that will be called by the client. For example:

<pre>
public class MyHub : Hub
{
    public async Task SendMessage(string user, string message)
    {
        await Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
</pre>

4. In your client-side code, you can use the SignalR JavaScript client library to call the server-side methods and receive notifications from the server. To use SignalR from the client-side, you will need to include the SignalR JavaScript library in your HTML page. You can do this by adding the following script tag to your page:

<pre><script src="/lib/signalr/signalr.js"></script></pre>

5. You can then use the following JavaScript code to create a connection to the SignalR hub and start receiving messages:

<pre>
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/myHub")
    .build();

connection.on("ReceiveMessage", (user, message) => {
    const encodedMsg = user + " says " + message;
    const li = document.createElement("li");
    li.textContent = encodedMsg;
    document.getElementById("messagesList").appendChild(li);
});

connection.start().catch(err => console.error(err.toString()));

</pre>

This code creates a new SignalR hub connection and sets up a handler for the "ReceiveMessage" event. When the server sends a message with the "ReceiveMessage" event, the client-side code will append a new list item to the "messagesList" element with the message content.

6. To send a message from the client to the server, you can call a server-side method using the invoke method:

<pre>connection.invoke("SendMessage", user, message).catch(err => console.error(err.toString()));</pre>
