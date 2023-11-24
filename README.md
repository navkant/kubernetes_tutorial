1. Set hostname using hostnamectl command
        `hostnamectl set-hostname <node-name>`


2. Add hosts name and their IPs in /etc/hosts/
        `<private_id> <hostname>`


3. Disable security for centos or redhat systems
        `setenforce 0`


4. Reboot


5. Install kubernetes, kubeadm, kubelet, kubectl.


6. create kubernetes repo in `/etc/yum.repos.d`
        add a file named `kubernetes.repo`

        `
        [kubernetes]
        name=Kubernetes
        baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
        enabled=1
        gpgcheck=1
        repo_gpgcheck=0
        gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
        `

7. `yum install -y docker kubelet-1.21.3 kubeadm-1.21.3 kubectl-1.21.3`


8. `systemctl start kubelet docker`


9. `systemctl enable kubelet docker`


10. Run this command on master node to intialize cluster, kubelet, control-plane etc
        `kubeadm init --apiserver-advertise-address=<master_node_private_ip> --pod-network-cidr=192.168.0.0/16`

    if using smaller machines then expected specifications
        `--ignore-preflight-errors=NumCPU --ignore-preflight-errors=Mem`

11. Run this command on master to generate join token
        `kubeadm token create --print-join-command`
    it will return something like this 
        `kubeadm join 172.31.44.134:6443 --token qu5fnw.dqn8vprvek5u8f4m --discovery-token-ca-cert-hash sha256:9641a78a4143a80c7e4c2e9c328ea84498236ca9c83a128ead68d15eb218ef3c`

    run this command on any worker node to join the cluster

12. On master node create .kube directory in home directory
        `mkdir -p $HOME/.kube`
    copy admin file from kubernetes conf to .kube/config
        `sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config`
    
    `sudo chown $(id -u):$(id -g) $HOME/.kube/config`


13. Run `kubectl get nodes` on master to see all nodes


14. Run `kubectl apply -f https://docs.projectcalico.org/v3.3/getting-started/kubernetes/installation/hosted/rbac-kdd.yaml`
