#1. Name
6thSense
#2. Author
Haroon Meer
#3. License, version & release date
License : GPLv2  
Version : v1.0  
Release Date : Unknown  
#4. Description
A while back antirez, in a post to Bugtraq, detailed a new Tcp portscan method.
This method allows one to portscan a host, using spoofed packets, while remaining totally invisible to the scanned host < almost as if u had a 6th sense ;) >.
The details of the scan (almost totally stolen from antirez's original post) works as follows...  

(A) When an open tcp port recieves a SYN, it replies with a SYN|ACK
When a closed tcp port recieves a SYN, it replies with a RST|ACK  

(B) When a host recieves an unknown SYN|ACK, it replies with a RST  
When a host recieves an unknown RST, it replies with nothing  

(C) You can tell the number of packets a host is sending by reading the ID value in the ip header  

What this means....  
We send 4 packets to our dummy host, to port 0, with no tcp flags set, and make a note of the incoming ip id's  

Scanning Dumb Host (for Dumbness)  
33144  
33145  
33146  
33147  

If the incoming id's do not show a consistent increase, the host is not dumb enough to suit our purposes, and the scan aborts.  
If the incoming id's show a constant single increment, then it is safe to assume that the dummy host is not actively talking/communicating to any other host (at this point in time)
We then send a spoofed packet (SYN) to our target host, on our target port, on behalf of our Dummy.  

We Have a consistant 1 increment host  

> Injecting Spoofed Packet

and once more track the incoming ip id's  

33148  
33150  
33152  
33156  

Now, if the target port was closed, it would have replied with a RST, < as mentioned in (A) earlier > and our Dummy would have responded with nothing < as mentioned in B >
But, if the target port was open, it would have replied with a SYN|ACK (A), causing our Dummy to reply with a RST. Dummy's ip id count, will now increase, as it has been forced into conversation with Target.  

> Yup looks like 22 is open on 196.10.XXX.38 
#5. Usage
Usage -d < dumb_host > -t < target > -s < start port > -f < Final port > -i < interface >
#6. Requirements
Net::RawIP, run ==> perl -MCPAN -e shell ==> install  
Net::RawIP   
