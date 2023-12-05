# Visualising 5G traces:

These instructions are for running on Mac OS X.

Install Wireshark per the [instructions](README.md) and link locally:

```
sudo ln -s /Applications/Wireshark.app/Contents/MacOS/tshark /usr/local/bin/tshark
sudo ln -s /Applications/Wireshark.app/Contents/MacOS/tshark wireshark/WiresharkPortable_4.2.0/App/Wireshark/tshark
```

Install Python libraries:

```
pip install -r requirements.txt
```

Grab the Free5GC core network function pods in to a file (assumes a local kubeconfig connection is configured to communicate with the the core master server 192.168.56.6):

```
kubectl get pods -n 5g -o yaml > pods.yaml
```

Run the visualiser to generate an SVG from the pcap 5G trace file (currently comprises only Free5GC core network function SBI interactions, plan to add GNB pcap traces too at some point):

```
python3 trace_visualizer.py -pods pods.yaml -wireshark "4.2.0" "traces/free5gc.pcap.pcapng"
```