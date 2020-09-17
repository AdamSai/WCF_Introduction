# Setting up SOAP with .NET WCF

## Setting up the server
1. Create a new project in Visual Studio 2019 and select the WCF Service Application template. If you do not have this follow this guide to quickly install it: [Installing WCF Templates](#installing_wcf_templates)  
![](https://i.imgur.com/Dhkhg5Q.png)
2. Inside the `IService1.cs` interface, add a new method called `Add`  
``` cs
    [ServiceContract]
    public interface IService1
    {

        [OperationContract]
        string GetData(int value);

        [OperationContract]
        CompositeType GetDataUsingDataContract(CompositeType composite);

        [OperationContract]
        int Add(int a, int b);
    }

```  
3. Inside the `Service1.svc` file, implement the new method.  
``` cs
    public class Service1 : IService1
    {
        public int Add(int a, int b)
        {
            return a + b;
        }

        public string GetData(int value)
        {
            return string.Format("You entered: {0}", value);
        }

        public CompositeType GetDataUsingDataContract(CompositeType composite)
        {
            if (composite == null)
            {
                throw new ArgumentNullException("composite");
            }
            if (composite.BoolValue)
            {
                composite.StringValue += "Suffix";
            }
            return composite;
        }
    }
```  
4. Start the application, and you now have your WCF server running :)
**Note:**
The WCF Test Client window will most likely pop up, and you can use it to test the methods in your WCF application. **Do not** close it, or the server will shut down as well.  

## Client
If you are running the server, open up a new Visual Studio 2019 process.  
1. Create a new project with the C# Console Application template  
![](https://i.imgur.com/wXbNmF9.png)
2. Right click on the project in the Solution Explorer and select Add -> Service Refference
![](https://i.imgur.com/EFF2ROu.png)
3. Select `Microsoft WCF Web Service Reference Provider`  
![](https://i.imgur.com/drrG59D.png)
4. Jump back into the WCF Test Client window and copy the adress of the server  
![](https://i.imgur.com/wy4YjLd.png)  
5. Paste the address into the URI field **and add ?wsdl to the end of the URI**
6. Press Go
7. Verify your `Add` method is in the services found
![](https://i.imgur.com/VLvudTQ.png)
8. Go to Client Options and enable `Generate Synchronous Operations`  
![](https://i.imgur.com/nLAMflr.png)
9. Press Finish
10. In the `Program.cs`, add the following code to the main method to call the SOAP method from the server:  
``` cs
            var client = new Service1Client();
            var a = 2;
            var b = 2;
            var result = client.Add(a, b);
            Console.WriteLine($"{a} + {b} = {result}");
```  
We're through!
![](https://i.imgur.com/gswRNXD.png)


## Installing WCF Templates
1. Open Visual Studio 2019.
2. Go to Tools -> Get Tools and 
3. In the window that pops up, go to Individual Components
4. Search for WCF
5. Tick the `Windows Communication Foundation feature`
6. Click modify and let it install
![](https://i.imgur.com/nLX5C8d.png)
You should now be able to create WCF applications.  
[Back to top](#Setting-up-the-server)
