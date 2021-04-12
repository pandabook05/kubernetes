参考文档:https://www.freesion.com/article/9238962685/


1.启动mds组件 （这里我们是在monitor节点加的，添加完后，可以看到有新增的进程出现）  
  ceph orch host label add mixed-vpc-gz-2048-sms-cephmon-55 mds  
  ceph orch host label add mixed-vpc-gz-2048-sms-cephmon-54  mds  
  ceph orch host label add mixed-vpc-gz-2048-sms-cephmon-53 mds  
  ceph orch apply mds cephfs label:mds  

2.创建cephfs文件系统  
  ceph osd pool create cephfs.k8s.meta  
  ceph osd pool create cephfs.k8s.data  
  ceph fs new cephfs.k8s cephfs.k8s.meta cephfs.k8s.data  

3.在K8S中创建存放CEPH ADMIN用户的SECRET,先登录monitor节点，查看秘钥  
  cd /etc/ceph  
  [root@mixed-vpc-gz-2048-sms-cephmon-53 ceph]# cat ceph.client.admin.keyring  
  [client.admin]  
      key = AQCSOGRgx7jtGBAAeJvv/VsqZ/dTxcn8jHKABg==  

  kubectl create secret generic ceph-admin --type="kubernetes.io/rbd" --from-literal=key=AQCSOGRgx7jtGBAAeJvv/VsqZ/dTxcn8jHKABg== --namespace=kube-system  
