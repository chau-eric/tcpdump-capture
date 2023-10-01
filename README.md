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
