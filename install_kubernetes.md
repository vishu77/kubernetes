





<!DOCTYPE html>
<html lang="en">
  <head>
 
  
 </head>

 
   
  <div id="readme" class="readme blob instapaper_body js-code-block-container">
    <article class="markdown-body entry-content" itemprop="text"><h1><a id="user-content-kubeadm--bring-your-own-nodes-byon" class="anchor" aria-hidden="true" href="#kubeadm--bring-your-own-nodes-byon"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Kubeadm : Bring Your Own Nodes (BYON)</h1>
<p>This documents describes how to setup kubernetes from scratch on your own nodes, without using a managed service. This setup uses <strong>kubeadm</strong> to install and configure kubernetes cluster.</p>
<h2><a id="user-content-compatibility" class="anchor" aria-hidden="true" href="#compatibility"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Compatibility</h2>
<p>Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications.</p>
<p>The below steps are applicable for the below mentioned OS</p>
<table>
<thead>
<tr>
<th>OS</th>
<th>Version</th>
<th>Codename</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Ubuntu</strong></td>
<td><strong>16.04</strong></td>
<td><strong>Xenial</strong></td>
</tr>
</tbody>
</table>
<h2><a id="user-content-base-setup-skip-if-using-vagrant" class="anchor" aria-hidden="true" href="#base-setup-skip-if-using-vagrant"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Base Setup</h2>
<p>On all nodes which would be part of this cluster, you need to do the base setup as described in the following steps. To simplify this, you could also   <a href="https://github.com/vishu77/kubernetes/installkube.sh">download and run this script</a></p>
<h3><a id="user-content-create-kubernetes-repository" class="anchor" aria-hidden="true" href="#create-kubernetes-repository"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Create Kubernetes Repository</h3>
<p>We need to create a repository to download Kubernetes.</p>
<pre><code>curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
</code></pre>
<pre><code>cat &lt;&lt;EOF &gt; /etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF
</code></pre>
<h3><a id="user-content-installation-of-the-packages" class="anchor" aria-hidden="true" href="#installation-of-the-packages"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Installation of the packages</h3>
<p>We should update the machines before installing so that we can update the repository.</p>
<pre><code>apt-get update -y
</code></pre>
<p>Installing all the packages with dependencies:</p>
<pre><code>apt-get install -y docker.io kubelet kubeadm kubectl kubernetes-cni
</code></pre>
<pre><code>rm -rf /var/lib/kubelet/*
</code></pre>
<h3><a id="user-content-setup-sysctl-configs" class="anchor" aria-hidden="true" href="#setup-sysctl-configs"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Setup sysctl configs</h3>
<p>In order for many container networks to work, the following needs to be enabled on each node.</p>
<pre><code>sysctl net.bridge.bridge-nf-call-iptables=1
</code></pre>
<p>The above steps has to be followed in all the nodes.</p>
<h2><a id="user-content-initializing-master" class="anchor" aria-hidden="true" href="#initializing-master"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Initializing Master</h2>
<p>This tutorial assumes <strong>kube-01</strong>  as the master and used kubeadm as a tool to install and setup the cluster. This section also assumes that you are using vagrant based setup provided along with this tutorial. If not, please update the IP address of the master accordingly.</p>
<p>To initialize master, run this on kube-01</p>
<pre><code>kubeadm init --apiserver-advertise-address 192.168.56.101(replace with your Ip Address) --pod-network-cidr=192.168.0.0/16 (replace with your Subnet)
</code></pre>
<h3><a id="user-content-initialization-of-the-nodes-previously-minions" class="anchor" aria-hidden="true" href="#initialization-of-the-nodes-previously-minions"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Initialization of the Nodes (Previously Minions)</h3>
<p>After master being initialized, it should display the command which could be used on all worker/nodes to join the k8s cluster.</p>
<p>e.g.</p>
<pre><code>kubeadm join --token c04797.8db60f6b2c0dd078 192.168.12.10:6443 --discovery-token-ca-cert-hash sha256:88ebb5d5f7fdfcbbc3cde98690b1dea9d0f96de4a7e6bf69198172debca74cd0
</code></pre>
<p>Copy and paste it on all node.</p>
<h5><a id="user-content-troubleshooting-tips" class="anchor" aria-hidden="true" href="#troubleshooting-tips"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Troubleshooting Tips</h5>
<p>If you lose  the join token, you could retrieve it using</p>
<pre><code>kubeadm token list
</code></pre>
<p>On successfully joining the master, you should see output similar to following,</p>
<pre><code>root@kube-03:~# kubeadm join --token c04797.8db60f6b2c0dd078 159.203.170.84:6443 --discovery-token-ca-cert-hash sha256:88ebb5d5f7fdfcbbc3cde98690b1dea9d0f96de4a7e6bf69198172debca74cd0
[kubeadm] WARNING: kubeadm is in beta, please do not use it for production clusters.
[preflight] Running pre-flight checks
[discovery] Trying to connect to API Server "159.203.170.84:6443"
[discovery] Created cluster-info discovery client, requesting info from "https://159.203.170.84:6443"
[discovery] Requesting info from "https://159.203.170.84:6443" again to validate TLS against the pinned public key
[discovery] Cluster info signature and contents are valid and TLS certificate validates against pinned roots, will use API Server "159.203.170.84:6443"
[discovery] Successfully established connection with API Server "159.203.170.84:6443"
[bootstrap] Detected server version: v1.8.2
[bootstrap] The server supports the Certificates API (certificates.k8s.io/v1beta1)

Node join complete:
* Certificate signing request sent to master and response
  received.
* Kubelet informed of new secure connection details.

Run 'kubectl get nodes' on the master to see this machine join.
</code></pre>
<h3><a id="user-content-setup-the-admin-client---kubectl" class="anchor" aria-hidden="true" href="#setup-the-admin-client---kubectl"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Setup the admin client - Kubectl</h3>
<p>On Master Node</p>
<pre><code>mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
</code></pre>
<h2><a id="user-content-installing-cni-with-weave" class="anchor" aria-hidden="true" href="#installing-cni-with-weave"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Installing CNI with Weave</h2>
<p>Installing overlay network is necessary for the pods to communicate with each other across the hosts. It is necessary to do this before you try to deploy any applications to your cluster.</p>
<p>There are various overlay networking drivers available for kubernetes. We are going to use <strong>Weave Net</strong>.</p>
<pre><code>
export kubever=$(kubectl version | base64 | tr -d '\n')
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$kubever"
</code></pre>
<h2><a id="user-content-validating-the-setup" class="anchor" aria-hidden="true" href="#validating-the-setup"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Validating the Setup</h2>
<p>You could validate the status of this cluster, health of pods and whether all the components are up or not by using a few or all of the following commands.</p>
<p>To check if nodes are ready</p>
<pre><code>kubectl get nodes
kubectl get cs

</code></pre>
<p>[ Expected output ]</p>
<pre><code>root@kube-01:~# kubectl get nodes
NAME      STATUS    ROLES     AGE       VERSION
kube-01   Ready     master    9m        v1.8.2
kube-02   Ready     &lt;none&gt;    4m        v1.8.2
kube-03   Ready     &lt;none&gt;    4m        v1.8.2
</code></pre>
<p>Additional Status Commands</p>
<pre><code>kubectl version

kubectl cluster-info

kubectl get pods -n kube-system

kubectl get events

</code></pre>
<p>It will take a few minutes to have the cluster up and running with all the services.</p>
<h3><a id="user-content-possible-issues" class="anchor" aria-hidden="true" href="#possible-issues"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Possible Issues</h3>
<ul>
<li>Nodes are node in Ready status</li>
<li>kube-dns is crashing constantly</li>



root@kube-01:~# kubectl logs kube-dns-545bc4bfd4-dh994  kubedns  -n kube-system
I1106 14:41:15.542409       1 dns.go:48] version: 1.14.4-2-g5584e04
I1106 14:41:15.543487       1 server.go:70] Using

....

</code></pre>
<h2><a id="user-content-enable-kubernetes-dashboard" class="anchor" aria-hidden="true" href="#enable-kubernetes-dashboard"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Enable Kubernetes Dashboard</h2>
<p>After the Pod networks is installled, We can install another add-on service which is Kubernetes Dashboard.</p>
<p>Installing Dashboard:</p>
<pre><code>kubectl apply -f https://gist.githubusercontent.com/initcron/32ff89394c881414ea7ef7f4d3a1d499/raw/4863613585d05f9360321c7141cc32b8aa305605/kube-dashboard.yaml

</code></pre>
<p>This will create a pod for the Kubernetes Dashboard.</p>
<p>To access the Dashboard in th browser, run the below command</p>
<pre><code>kubectl describe svc kubernetes-dashboard -n kube-system
</code></pre>
<p>Sample output:</p>
<pre><code>kubectl describe svc kubernetes-dashboard -n kube-system
Name:                   kubernetes-dashboard
Namespace:              kube-system
Labels:                 app=kubernetes-dashboard
Selector:               app=kubernetes-dashboard
Type:                   NodePort
IP:                     10.98.148.82
Port:                   &lt;unset&gt; 80/TCP
NodePort:               &lt;unset&gt; 31000/TCP
Endpoints:              10.40.0.1:9090
Session Affinity:       None
</code></pre>
<p>Now check for the node port, here it is 31000, and go to the browser, and access the dashboard with the following URL
<code>do not use the IP above, use master node IP instead</code></p>
<pre><code>http://NODEIP:31000
</code></pre>
  </body>
</html>

