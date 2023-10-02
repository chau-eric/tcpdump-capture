<h1>Analyze Network Traffic with tcpdump</h1>

<h2>Description</h2>
This project consists of using tcpdump to capture and analyze TCP packets. We will also create a shell script with options and advanced expressions to filter the packets captured.
<br />


<h2>Tools Used</h2>

- <b>tcpdump</b>
- <b>Wireshark</b>
- <b>Visual Studio Code</b> 

<h2>Environment</h2>

- <b>Ubuntu 18.04.4 LTS</b>

<h2>Project walk-through:</h2>

<h4>Perform first tcpdump capture</h4>
<p align="center">
<img width="697" alt="Untitled" src="https://github.com/chau-eric/tcpdump-capture/assets/76719902/a881084b-5287-4a99-8e24-f7a8de461249"><br/>
</p>
This first capture is just to check that our network is up and running. By default you need to be a super user to use tcpdump, therefore the command "sudo" needs to be used. The -c option is used here so we don't infinitely capture traffic and overflow our screen with captures. 10 is being passed to -c to tell tcpdump that we only want to capture 10 packets.
<br />
<br />

<h4>Capture traffic between a target host</h4>
<p align="center">
<img width="544" alt="Untitled" src="https://github.com/chau-eric/tcpdump-capture/assets/76719902/a75332a5-d823-4db6-bc6a-beadcfa4e51f">
</p>
Now we add even more options to further filter our results. First, -# will number each packet captured. This will make it easier to differentiate between individual packets. The -tttt option will display the packet timestamps in a more human-readable format. The "host" option indicates that we want to only capture traffic between the specified host, which in this case is google.com You can see that initially, no packets are captured with these options. This is because no packets are being sent between our network and google.com. However after opening up Google in our web browser, we finally get some captures.<br/><br/>
<p align="center">
<img width="696" alt="Untitled" src="https://github.com/chau-eric/tcpdump-capture/assets/76719902/1b8ebfb3-d045-4aea-9ab2-6c87aa0a6206">
</p>
<br />

<h4>Creating packet capture files with size and time limits</h4>
Now let's say we want to store the captured packets in a file for later access or to analyze in another tool. We can use the -w option to write the captures to a specified .pcap file to accomplish this, which in this case is named captured.pcap. However, let's say we only want to see traffic within the last 10 minutes, or 600 seconds. The -G option will wipe the contents of the file after the specified seconds have passed. We should also be careful of our .pcap file from becoming too large. The -C option allows us to specify the maximum file size, in million bytes, that we want the file to be. In this case, let's say we don't want our file to be more than 1 million bytes. Now with these options, we no longer have to limit our packet captures by number of packets, so we can remove the -c option completely. We also want to use the -XX option to format the packet contents in hexadecimal and ASCII representation for later analysis. <br/><br/>
<p align="center">
<img width="698" alt="Untitled" src="https://github.com/chau-eric/tcpdump-capture/assets/76719902/91280474-0d05-4703-8258-d0149f0c5a37">
</p>
Note how none of the packets are output to the terminal. This is because all the information is being written to captured.pcap instead.

<h4>Advanced expressions</h4>
What if the previous options still aren't enough? tcpdump allows the use of expressions for more advanced filtering.<br/><br/><p align="center">
<img width="697" alt="Untitled" src="https://github.com/chau-eric/tcpdump-capture/assets/76719902/169bcd75-2c58-4843-a630-bb31aa964f25"></p>
Let's break down the expression in the single quotes. First, we select the 12th byte of the tcp header. Of that byte, we're going to shift the upper 4 bits back by 4. Then, we multiply it by 4 which causes those bits to shift towards their original position by 2. Altogether, this results in a net shift of 2 of the original upper 4 bits. 0x47455420 represents "GET" in hexadecimal. So this whole expression basically means we want to capture all the TCP traffic for a GET operation. In order to actually generate GET traffic to capture, I sent a GET request to a simple API. The API used should not matter, but I chose Recipe Puppy API for this example. But all these packets should only be TCP packets using the GET operation. Let's modify the command to write to a file and resend the GET request so we can further analyze the results.
<br/><br/><p align="center">
<img width="696" alt="Untitled" src="https://github.com/chau-eric/tcpdump-capture/assets/76719902/7e0fb2ac-2656-47b2-a67b-5221d939b974">
</p>
The packet analyzer I will be using is Wireshark. Upon opening advanced.pcap in Wireshark, we can see that only one packet was captured.
<br/><p align="center">
<img width="656" alt="Untitled" src="https://github.com/chau-eric/tcpdump-capture/assets/76719902/864a4f0f-ed60-40cf-86dc-7647b2dd7008">
</p>
And when we inspect the information stored in the header, we can confirm that it did use the GET operation and our expression filtered the traffic correctly.

<h4>Create and execute the shell script</h4>
Now that we confirmed that our expression and options are correct, let's put the command in a shell script for easy execution in the future. In VSCode, I simply created a new file called advanced.sh and copy-and-pasted our command from the terminal.<br/><p align="center">
<img width="697" alt="Untitled" src="https://github.com/chau-eric/tcpdump-capture/assets/76719902/1e5792c9-7f59-418d-8a06-85d17ea621b7">
</p>
However, we can see that this is not yet an executible. To do this, we simply need to add the executible permission to the file using chmod.
<br/><p align="center">
<img width="699" alt="Untitled" src="https://github.com/chau-eric/tcpdump-capture/assets/76719902/81ccc9b0-75ef-4c9b-bec1-a1a35242ee6b">
</p>
And the project is finished! We can now executed advanced.sh in the terminal to capture GET tcp traffic.
<br/><p align="center">
<img width="697" alt="Untitled" src="https://github.com/chau-eric/tcpdump-capture/assets/76719902/a6beb9b3-3276-4f5a-9c2c-45743903bebd">
</p>
