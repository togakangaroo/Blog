How many times have I run into this?

A css error that is only happening on the tablet and me with no [Browserstack](http://www.browserstack.com/) account. 
Js errors [I can handle](http://jsconsole.com/) and heck, I even know how to [set up a pseudo remote-debugging environment 
to the tablet](http://people.apache.org/~pmuellr/weinre/docs/latest/Installing.html). But I still need to open up
my webserver to off-machine requests and love it as I do, [ngrok](https://ngrok.com) is not going to fit this bill.

Since I'm in Microsoft-world usually running IIS Express in my dev environment this requires a whole lot of messing around with
unfriendly and undocumented configurations. 

I run all my development tools in a virtual machine so the following is my process to do this. If you do not, just take out the virtual machine and port forwarding parts.

1. Determine the port number you want to access your application on. In my case it's 1352 and I make sure its hard coded to that in my web project properties. While in there (under the Web section) you might want to make sure that you're indeed using iisexpress
1. Clean up your netsh urlacl. In an admin powershell console: 

       netsh http show urlacl | select-string 1352

   for each one of these you will want to run a delete
   
       netsh http delete urlacl url=http://each-listed-ip:1352/
       
1. Allow wildcard urlacl

       netsh http add urlacl url=http://*:1352/ user=everyone
       
1. Setup a wildcard iis express binding in your iis applicationhosts file. Usually in `~\Documents\IISExpress\config\applicationhost.config`.
   Find the xml node corresponding to your project. Something along the lines of
   
           ```xml
            <site name="YourProject.Web" id="11">
                <application path="/" applicationPool="Clr4IntegratedAppPool">
                    <virtualDirectory path="/" physicalPath="c:\yourproject\YourProject.Web" />
                </application>
                <bindings>
                    <binding protocol="http" bindingInformation="*:1352:localhost" />
                </bindings>
            </site>
            ```
            
    modify the binding node to a wildcard:
    
        <binding protocol="http" bindingInformation="*:1352:" />

1. At this point you should be able to access your site using http://your-ip-address:1352 from the same machine that hosts your iis express. 

1. Open up your firewall. Go to Windows Firewall and to the Advanced Settings. Create a new Inbound Rule allowing through your port.
