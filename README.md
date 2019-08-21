# Route Configuration

This repository is a lab for NCTU course "Introduction to Computer Networks 2018".

---
## Abstract

In this lab, we are going to write a Python program with Ryu SDN framework to build a simple software-defined network and compare the different between two forwarding rules.

---
## Objectives

1. Learn how to build a simple software-defined networking with Ryu SDN framework
2. Learn how to add forwarding rule into each OpenFlow switch

---
## Execution

> TODO:
> * How to run your program?
1. Open pietty and connect to **`140.113.195.69 port 45078`**
2. Log in as **`root`** with the password **`cn2018`**
3. Navigate to the Route_Configuration folder containing `topo.py` and `controller.py` through the command `cd Route_Configuration/src`
4. Repeat **steps 1-3** to open a new terminal

> Part One - SimpleController
5. In the __first terminal__, execute `topo.py` with Mininet through the command `mn --custom topo.py --topo topo --link tc --controller remote`
6. Once Mininet is successfully executed in the *first terminal*, in the __second terminal__, execute `SimpleController.py` with Ryu manager through the command `ryu-manager SimpleController.py --observe-links`
7. Once Ryu manager is succesfully executed, use iPerf to measure the bandwidth in the network, and save the results to **`./out`** folder through the following commands:
  - `h1 iperf -s -u -i 1 -p 5566 > ./out/result1 &`
  - `h2 iperf -c 10.0.0.1 -u -i 1 -p 5566`
8. Wait for iPerf to finish running for the results to be displayed and saved
9. In the **first terminal**, exit Mininet through the command `exit` and wait for Mininet to shut down
10. In the **second terminal**, exit Ryu manager by pressing `ctrl z`
11. In the **first terminal**, clean RTNETLINK through the command `mn -c`

> Part Two - controller
12. In the __first terminal__, execute `topo.py` with Mininet through the command `mn --custom topo.py --topo topo --link tc --controller remote`
13. Once Mininet is successfully executed in the *first terminal*, in the __second terminal__, execute `controller.py` with Ryu manager through the command `ryu-manager controller.py --observe-links`
14. Once Ryu manager is succesfully executed, use iPerf to measure the bandwidth in the network, and save the results to **`./out`** folder through the following commands:
  - `h1 iperf -s -u -i 1 -p 5566 > ./out/result2 &`
  - `h2 iperf -c 10.0.0.1 -u -i 1 -p 5566`
15. Wait for iPerf to finish running for the results to be displayed and saved
16. In the **first terminal**, exit Mininet through the command `exit` and wait for Mininet to shut down
17. In the **second terminal**, exit Ryu manager by pressing `ctrl z`
18. In the **first terminal**, clean RTNETLINK through the command `mn -c`


> * What is the meaning of the executing command (both Mininet and Ryu controller)?
1. `mn --custom topo.py --topo topo --link tc --controller remote`
  - `mn`: execute Mininet in the current terminal
  - `--custom topo.py`: define a custom topology `topo.py` to be used as the default topology
  - `--topo topo`: creates a new Topo class `topo` to be used
  - `--link tc`: specify the link `tc` to be used
  - `--controller remote`: specify a external OpenFlow controller that is running remotely in another terminal to be used

2. `ryu-manager controller.py --observe-links`
  - `ryu-manager`: execute Ryu in the current terminal
  - `controller.py`: specifies the python file from which the custom Ryu controller is to be used
  - `--observe-links`: displays link discovery events to show a list of links present in the network


> * Show the screenshot of using iPerf command in Mininet (both `SimpleController.py` and `controller.py`)
1. `SimpleController.py`:

  ![simpletest.png](https://github.com/f0urseas0ns/networks-lab-3/blob/master/screenshots/result%201.PNG)


2. `controller.py`:

  ![simpletest.png](https://github.com/f0urseas0ns/networks-lab-3/blob/master/screenshots/result%202.PNG)

---
## Description

### Tasks

> TODO:
> * Describe how you finish this work in detail

1. Environment Setup
  - I first connected to my container via Secure Shell (SSH) through PieTTY. Using the user and password credentials **`root`** and **`cn2018`**, I connected to IP address **`140.113.195.69`** on **`port 45078`**. Once I was successfully logged onto my container, I cloned my working GitHub repository to a new directory called **`"Route_Configuration"`**. This was done using the command `git clone https://github.com/f0urseas0ns/networks-lab-3.git`. To confirm that my files were cloned successfully, I checked that the correct directory with right files existed using the command `ls /root/`.

  - Once I have successfully cloned the GitHub repository, I then checked that Mininet was running properly. This was done through the command `sudo mn` which opens Mininet in the terminal. An error message appeared saying that there was an error in connecting to Open vSwith database server. Since this was an error due to Open vSwitch service not being started, this error was easily fixed with the command `open vswitch-switch start` which starts the affected service.

2. Example of Ryu SDN
  - To run Ryu manageer, a second terminal is required. Thus, I opened a new session to my container using the aforementioned SSH methods and credentials. In the first terminal that I had opened, I then navigated to the **src** folder of **Route_Configuration** through the command `cd /root/Route_Configuration/src/`. Once I was in the **src** folder, I then executed `SimpleTopo.py` with Mininet through the command `mn --custom SimpleTopo.py --topo topo --link tc --controller remote`. Once the network is created and the Mininet CLI has started, I then switched over to the second terminal.
  - In the second terminal, I again navigated to the **src** folder of **Route_Configuration** through the same command as before. This time, I executed `SimpleController.py` with Ryu manager through the command `ryu-manager SimpleController.py --observe-links`. Once Ryu manager has successfully ran, I could then measure the performance of the example network with iPerf.
  - Once I was done, I then exited Mininet CLI in the first terminal using `exit`; in the second terminal, I exited Ryu manager by pressing `ctrl z`. Once I had exited both programs, I then proceeded to clear up `RTNETLINK` through the command `mn -c`, which ensures that all the switches, links and controller are cleared up.

3. Mininet Topology
  - To define my own custom topology, I first duplicated `SimpleTopo.py` to create `topo.py` through the command `cp SimpleTopo.py topo.py`. This created a copy of `SimpleTopo.py` and renamed it to the corresponding name that I wanted. This command also ensured that `topo.py` will be created within the same working directory as the other python files, ensuring that there would be no error when trying to run `topo.py` with the other controllers.
  - Once I was done creating `topo.py`, I then modified the file to include the constaints that had been specified in the path **`/Route_Configuration/src/topo/topo.png`**. Namely, the constraints that were added included bandwidth, delay and loss rate between the different switches: s1-s2, s1-s3 and s2-s3. No further constraints were added to the links between the hosts and switches: h1-s1 and h2-s3.
  - Once `topo.py` was modified, I then tested that this new custom topology would be able to run successfully. This was done by executing `topo.py` with Mininet through the command `mn --custom topo.py --topo topo --link tc --controller remote`. After executing Ryu manager, I should see that there is no resulting error messages, indicating that I have indeed modified `typo.py` successfully. The resulting network can also be further tested in the Mininet CLI through the command `h1 ping h2`, which allows the two hosts to ping each other to check for their connectivity.

4. Ryu Controller
  - Once I was done with `topo.py`, I then moved on to duplicate `SimpleController.py` to create `controller.py`, using a similar command as before `cp SimpleController.py controller.py`. Going through the code of `SimpleController.py`, it can be seen that I am only required to modify `def switch_features_handler(self, ev):` in order to define my own forwarding rules. The forwarding rules that I am required to have could be found in page 35 of the lab handout.
  - Comparing the forwarding rules of `SimpleController.py` and the required forwarding rules of `controller.py`, it can be seen that in `SimpleController.py` the only forwarding takes place between h1-s1, s1-s3, and s3-h2. In contrast, forwarding in `controller.py` takes place between h1-s1, s1->s3, s3-h2, s3->s2 and s2->s1 (where -> indicates direction of forwarded packets). Since `SimpleController.py` lacks forwaridng rules for s2, I created a generic forwarding rule based on the forwarding rules of s1 in `SimpleController.py`. I then modified the forwarding rules of s2 to accept incoming packets of s3 on **port 2** and to forward these incoming packets to s1 through **port 1**. This is done through the lines `in_port=2` which check **port 2** for incoming packets and `actions = [parser.OFPActionOutput(1)]` which forward the packets through **port 1**.

  ![s2.png](https://github.com/f0urseas0ns/networks-lab-3/blob/master/screenshots/s2.PNG)

  - Once I was done creating and modifying the forwarding rules of s2, I modified forwarding rules in s1. In `topo.py`, s1 should now only be sending outgoing packets to s3 received on **port 1** from h1, but received incoming packets from s2 instead of s3. Thus, I modified the forwarding rules of s1 to receive incoming packets from s2 on **port 2** and redirect these incoming packets to h1 through **port 1**. This is done through the lines `in_port=2` which check **port 2** for incoming packets and `actions = [parser.OFPActionOutput(1)]` which forward the packets through **port 1**.

  ![s1.png](https://github.com/f0urseas0ns/networks-lab-3/blob/master/screenshots/s1.PNG)

  - The final modification to forwarding rules I had to make was for s3. In `topo.py`, s3 should now receive incoming packets from s1 and forward them to h2. s3 will then wait for incoming packets from h2 on **port 1** but instead of forwarding these incoming packets to s1, s3 will now forward these packets to s2 through **port 3**. This is done through the lines `in_port=1` which check **port 1** for incoming packets and `actions = [parser.OFPActionOutput(3)]` which forward the packets through **port 3**.

  ![s3.png](https://github.com/f0urseas0ns/networks-lab-3/blob/master/screenshots/s3.PNG)

  - Once I had finished modifying `controller.py`, I then checked that it could work properly as intended. This is done by running `topo.py` again with Mininet, but instead of running `SimpleController.py`, this time I ran `controller.py` with Ryu manager instead. Once I confirmed that the network is running properly, I then proceeded with measuring the network performance with iPerf.

5. Measurement
   - The objective of this lab is to measure the performance of the network when employing different forwarding rules, namely `SimpleController.py` and `controller.py`. iPerf is a flexible tool that allows for active measurements of maximum achievable bandwidths on IP networks, while supporting the tuning for various parameters for varied purposes. In this case, we used it to determine the results of the network topology by returning the bandwidth and percentage loss over an interval of 1s for up to 10s.
   - In the iPerf tests, the iPerf server is configured to be hosted on h1 while the iPerf client connects to the server from h2. h2 will connect to the iPerf server at **10.0.0.1** on **port 5566**. When a connection has been successfully established between the client and the server, 1470 byte datagrams are then sent to the client while the performance of the network is recorded. Once the datagram has finished sending after 10 seconds, the results will be displayed where the performance can be seen and compared. This process is repeated twice, once for either controller and the results saved to **`Route_Configuration/src/out/result1`** and **`Route_Configuration/src/out/result2`** respectively.

### Discussion

> TODO:
> * Answer the following questions

1. Describe the difference between packet-in and packet-out in detail.
    Packet-in message allows the switch to send a captured packet to the controller, allowing the controller to take possesion of the captured packet. For every packets forwarded to the controller reserved port using a flow entry or table-miss flow entry, a packet-in event is always sent to the controller to allow for possession of the packets. Packet-in events may also be generated during other processing such as TTL check.

    Packet-out are used by the controller to send packets out of a specified port on the switch and to forward packets received via packet-in messages. The packet-out messages must contain either a full packet or a buffer ID that references a packet stored in the switch. A list of actions contained within the message is also applied in the order they are specified, and which an empty action list will drop the packet.

    Packet-in is initiated by the switch for the current controller to take control of the packet while Packet-out is initiated by the current controller for the next controller to take control of the packet.

2. What is “table-miss” in SDN?
    A table-miss occurs when there is no match in the flow table found for the entry. The table configuration determines the behavior - a table miss flow entry can specify how to process unmatched packets. This could include dropping the packet, passing the packet to another table, or sending them to controllers via packet-in messages.

    Every flow table must support a table-miss flow entry in order to process the table-miss. In the case where no such table-miss flow entry exists, the packet is dropped.

3. Why is "`(app_manager.RyuApp)`" adding after the declaration of class in `controller.py`?
    `(app_manager.RyuApp)` allows the creation of `controller.py` as a subclass of `ryu.base.app_manager.RyuApp`. This done by defining `controller.py` to be a python module that acts as a Ryu application.

4. Explain the following code in `controller.py`.
    ```python
    @set_ev_cls(ofp_event.EventOFPPacketIn, MAIN_DISPATCHER)
    ```
    `set_ev_cls()` decorator sets the decorated method to become an event handler and determines when the decorator function should be called by the RyuApp.
    
    `ofp_event.EventOFPPacketIn` is the ev_cls argument in this decorator that RyuApp wants to receive, and specifies the type of event that is wanted, which is Packet-in.

    `CONFIG_DISPATCHER` is the dispatchers argument that specifies the negotiation phase for which the event should be generated. In this decorator, the `MAIN_DISPATCHER` negotiation phase is specified, thus the switch-features message will be received and a set-config message sent.

5. What is the meaning of “datapath” in `controller.py`?
    `datapath` refers to the switches in the topology being used with OpenFlow. These switches are s1, s2 and s3 which have the corresponding id number as their names.

6. Why need to set "`ip_proto=17`" in the flow entry?
    `ip_proto = 17` specifies User Datagram Protocol (UDP) to be used by the datagram. This is necessary as during bandwidth measurement of the network with iPerf, the measurement used UDP instead of TCP to measure the results ot the bandwidth. This was specified within the iPerf command line argemtns `-u`.
   
7. Compare the differences between the iPerf results of `SimpleController.py` and `controller.py` in detail.
    Using `SimpleController.py`, the iPerf results obtained is 26/893 or 2.9% loss in datagrams sent with execution time of 0.007ms. The network bandwidth measured is 1.02 Mbits/sec.

    Using `controller.py`, the iPerf results obtained is 11/893 or 1.2% loss in datagrams sent with execution time of 0.008ms. The network bandwidth measured is 1.04 Mbits/sec.

8. Which forwarding rule is better? Why?
    Rule 2 is a better forwarding rule as there is significant improvement in performance. The loss in datagram using rule 2 sees at least 50% decrease in loss of datagrams and a slightly higher bandwidth of 0.2 Mbits/sec. Althogh there is a slight increase in execution time of 0.001ms when using rule 2, it can be assumed to be negligible in affecting the performance since the network still has an overall improvement in performance.
    
---
## References

> TODO: 
> * Please add your references in the following


* **Ryu SDN**
    * [Ryubook Documentation](https://osrg.github.io/ryu-book/en/html/)
    * [Ryubook [PDF]](https://osrg.github.io/ryu-book/en/Ryubook.pdf)
    * [Ryu 4.30 Documentation](https://github.com/mininet/mininet/wiki/Introduction-to-Mininet)
    * [Ryu Controller Tutorial](http://sdnhub.org/tutorials/ryu/)
    * [OpenFlow 1.3 Switch Specification](https://www.opennetworking.org/wp-content/uploads/2014/10/openflow-spec-v1.3.0.pdf)
    * [Ryubook 說明文件](https://osrg.github.io/ryu-book/zh_tw/html/)
    * [GitHub - Ryu Controller 教學專案](https://github.com/OSE-Lab/Learning-SDN/blob/master/Controller/Ryu/README.md)
    * [Ryu SDN 指南 – Pengfei Ni](https://feisky.gitbooks.io/sdn/sdn/ryu.html)
    * [OpenFlow 通訊協定](https://osrg.github.io/ryu-book/zh_tw/html/openflow_protocol.html)
* **Python**
    * [Python 2.7.15 Standard Library](https://docs.python.org/2/library/index.html)
    * [Python Tutorial - Tutorialspoint](https://www.tutorialspoint.com/python/)
* **Others**
    * [Cheat Sheet of Markdown Syntax](https://www.markdownguide.org/cheat-sheet)
    * [Vim Tutorial – Tutorialspoint](https://www.tutorialspoint.com/vim/index.htm)
    * [鳥哥的 Linux 私房菜 – 第九章、vim 程式編輯器](http://linux.vbird.org/linux_basic/0310vi.php)

---
## Contributors

> TODO:
> * Please replace "`YOUR_NAME`" and "`YOUR_GITHUB_LINK`" into yours

* [Thomas Ng](https://github.com/f0urseas0ns)
* [David Lu](https://github.com/yungshenglu)

---
## License

GNU GENERAL PUBLIC LICENSE Version 3