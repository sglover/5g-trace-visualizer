# Setup

These instructions are for running on Mac OS X.

Install Wireshark per the [instructions](README.md) and link locally:

```
sudo ln -s /Applications/Wireshark.app/Contents/MacOS/tshark /usr/local/bin/tshark
sudo ln -s /Applications/Wireshark.app/Contents/MacOS/mergecap /usr/local/bin/mergecap
sudo ln -s /Applications/Wireshark.app/Contents/MacOS/tshark wireshark/WiresharkPortable_4.2.0/App/Wireshark/tshark
```

```
source ~/miniconda/bin/activate
conda activate llm
```

Install Python libraries:

```
pip install -r requirements.txt
```

# Running

```
vagrant/vagrant@192.168.56.6
./core-trace.sh

vagrant/vagrant@192.168.56.20
./gnb-trace.sh

vagrant/vagrant@192.168.56.21
./ue-trace.sh

vagrant/vagrant@192.168.56.21
./ue-trace1.sh
```


```
sshpass -p "vagrant" scp vagrant@192.168.56.20:/tmp/free5gc/gnb-eth2.pcap traces/
sshpass -p "vagrant" scp vagrant@192.168.56.21:/tmp/free5gc/ue-eth2.pcap traces/
sshpass -p "vagrant" scp vagrant@192.168.56.21:/tmp/free5gc/ue-tun.pcap traces/
```


<!-- On 100.65.5.68:

```
sshpass -p "vagrant" scp vagrant@192.168.56.20:/tmp/free5gc/gnb-eth1.pcap traces/
sshpass -p "vagrant" scp vagrant@192.168.56.21:/tmp/free5gc/ue-eth1.pcap /tmp/free5gc
``` -->


```
mergecap ../5gtraces/traces/*.pcap -w ../5gtraces/traces/free5gc.pcap.pcapng
```

# Visualising 5G traces

Grab the Free5GC core network function pods in to a file (assumes a local kubeconfig connection is configured to communicate with the the core master server 192.168.56.6):

```
kubectl get pods -n 5g -o yaml > pods.yaml
```

Run the visualiser to generate an SVG from the pcap 5G trace file (currently comprises only Free5GC core network function SBI interactions, plan to add GNB pcap traces too at some point):

```
python3 trace_visualizer.py -pods ../5gtraces/pods.yaml -openstackservers ../5gtraces/servers.yaml -wireshark "4.2.0" -http2ports "32445,5002,5000,32665,80,32077,5006,8080,3000,8000" -simple_diagrams True -show_timestamp True ../5gtraces/traces/simplediagrams/free5gc.pcap.pcapng

python3 trace_visualizer.py -pods ../5gtraces/pods.yaml -openstackservers ../5gtraces/servers.yaml -wireshark "4.2.0" -http2ports "32445,5002,5000,32665,80,32077,5006,8080,3000,8000" -show_timestamp True ../5gtraces/traces/fulldiagrams/free5gc.pcap.pcapng

python3 trace_visualizer.py -pods ../5gtraces/pods.yaml -openstackservers ../5gtraces/servers.yaml -wireshark "4.2.0" -http2ports "32445,5002,5000,32665,80,32077,5006,8080,3000,8000" -diagrams k8s_pod,k8s_namespace -show_timestamp True ../5gtraces/traces/diagrams/free5gc.pcap.pcapng

python3 trace_visualizer.py -pods ../5gtraces/pods.yaml -openstackservers ../5gtraces/servers.yaml -wireshark "4.2.0" -http2ports "32445,5002,5000,32665,80,32077,5006,8080,3000,8000" -diagrams k8s_pod,k8s_namespace -show_timestamp True ../5gtraces/traces/slimdiagrams/free5gc.pcap.pcapng
```

# Backup

```
tshark -r ../5gtraces/output2/free5gc.pcap.pcapng -2 -o "nas-5gs.null_decipher: TRUE" -Y "http2 or ngap or nas-5gs or pfcp or gtpv2 or diameter or radius or gtpprime or icmp or icmpv6.type == 134" -T pdml -J "http2 ngap pfcp gtpv2 tcp diameter radius gtpprime icmp icmpv6" -n -w ../5gtraces/output2/free5gc.pcap_4.2.0.pdml









tshark -r ../5gtraces/output2/free5gc.pcap.pcapng -2 -o "nas-5gs.null_decipher: TRUE" -Y "(http2 and (http2.type == 0 || http2.type == 1)) or ngap or nas-5gs or http or pfcp or gtpv2 or diameter or radius or gtpprime or icmp or icmpv6.type == 134" -T pdml -J "http2 ngap pfcp gtpv2 tcp diameter radius gtpprime icmp icmpv6 http" -n > ../5gtraces/output2/free5gc.pcap_4.2.0.pdml






tshark -r ../5gtraces/output2/free5gc.pcap.pcapng -2 -o "nas-5gs.null_decipher: TRUE" -T pdml -J "http2 ngap pfcp gtpv2 tcp diameter radius gtpprime icmp icmpv6 http" -n > ../5gtraces/output2/free5gc.pcap_4.2.0.pdml






python3 trace_visualizer.py -pods ../5gtraces/pods.yaml -openstackservers ../5gtraces/servers.yaml -wireshark "4.2.0" -http2ports "32445,5002,5000,32665,80,32077,5006,8080,3000,8000" -show_timestamp True ../5gtraces/output2/free5gc.pcap.pcapng








python3 trace_visualizer.py -pods ../5gtraces/pods.yaml -openstackservers ../5gtraces/servers.yaml -wireshark "4.2.0" -show_timestamp True ../5gtraces/output2/free5gc.pcap.pcapng



python3 trace_visualizer.py -pods ../5gtraces/pods.yaml -openstackservers ../5gtraces/servers.yaml -wireshark "4.2.0"  -show_timestamp True ../5gtraces/output1/udmsbi.pcap


python3 trace_visualizer.py -pods ../5gtraces/pods.yaml -openstackservers ../5gtraces/servers.yaml -wireshark "4.2.0" -http2ports "32445,5002,5000,32665,80,32077,5006,8080,3000,8000" -show_timestamp True ../5gtraces/output2/free5gc.pcap.pcapng

python3 trace_visualizer.py -pods ../5gtraces/pods.yaml -openstackservers ../5gtraces/servers.yaml -wireshark "4.2.0" -http2ports "2000,2001,2152,8000,8805,27017,33586,34970,34496,35130,35132,35150,33594,38382,38562,38576,38082,38086,38100,38114,38118,39340,39350,39364,40900,41752,41908,41914,44350,44366,44382,45110,45126,45132,45886,48414,48420,48432,48500,48506,48510,48706,51700,51710,51718,51728,54574,54590,54606,54618,54634,54644,54652,54666,54680,54684,54690,54702,54716,54728,54732,54736,54744,54754,54760,54768,54774,54786,54800,54816,54822,54836,54840,54850,58848,58860" -show_timestamp True ../5gtraces/output1/free5gc.pcap.pcapng

python3 trace_visualizer.py -pods ../5gtraces/pods.yaml -openstackservers ../5gtraces/servers.yaml -wireshark "4.2.0" -http2ports "2000, 2001, 2152, 8000, 8805, 27017, 33586, 34970, 34496, 35130, 35132, 35150, 33594, 38382, 38562, 38576, 38082, 38086, 38100, 38114, 38118, 39340, 39350, 39364, 40900, 41752, 41908, 41914, 44350, 44366, 44382, 45110, 45126, 45132, 45886, 48414, 48420, 48432, 48500, 48506, 48510, 48706, 51700, 51710, 51718, 51728, 54574, 54590, 54606, 54618, 54634, 54644, 54652, 54666, 54680, 54684, 54690, 54702, 54716, 54728, 54732, 54736, 54744, 54754, 54760, 54768, 54774, 54786, 54800, 54816, 54822, 54836, 54840, 54850, 58848, 58860" -show_timestamp True -simple_diagrams True ../5gtraces/output/free5gc.pcap.pcapng
```