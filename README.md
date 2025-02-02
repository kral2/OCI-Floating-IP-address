# OCIfloatingIP
Moving all IP secondary addresses from VNICs of VM#1 to VNICs of VM2#

Version 0.9

Author: stephan.alluchon@oracle.com

Oracle Cloud  Infrastructure (OCI)

The guide in this directory and accompanied files are released under an as-is, best effort, support policy. These scripts should be seen as community supported. 

We have 2 VM: VM1 and VM2. ALL Secondary privateIP will move from VNIC VM1 to VNIC VM2.

We use flask on default port 5000 for HTTP server.

When we receive a specific URL
we move a secondary private IP to a different VNIC in the same subnet
So the HTTP request is the trigger
URLs are:
http://@IP:5000/PrimaryIsVM1 ==> force VM1 to be primary for all PrivateIP (Ip secondary)

http://@IP:5000/PrimaryIsVM2 ==> force VM2 to be primary

http://@IP:5000/IamPrimary ==> The one which sends the request becomes Primary

http://@IP:5000/ItisPrimary ==> The one which sends the request becomes secondary

So the HTTP request is the trigger.
API reference
https://docs.cloud.oracle.com/iaas/api/#/en/iaas/20160918/PrivateIp/

The goal is to manage High Availability for Third party services such as PaloAlto, F5...

For this, we use OCI SDK and Flask/waitress server.

We also use a JSON file which is a configuration file : oci_value_IP_address.json

In this configuration file, we provide IP addresses of the 2 VM which are sending simple HTTP request. PaloAlto Networks VM are able to send an HTTP request in case they need to do move from active to standby VM.

The Python script analyzes IP source address of the request. Based on the IP source, it will move from VM1 to VM2 or the other side.

You can test your JSON file and be sure that you are using the right IP addresses with the python scripts:
test_json_ip_address_SDK.py
