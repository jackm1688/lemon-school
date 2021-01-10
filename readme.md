# 手动管理crd资源<br>

### 创建具有管理权限的用户，然后将本地的.kube/config文件copy到macos机器上 ~/.kube/configopenssl genrsa -out lemon-admin.key 2048 <br>
openssl req -new -key lemon-admin.key -subj "/CN=lemon-admin/O=system:masters" -out lemon-admin.csr<br>
openssl x509 -req -in lemon-admin.csr -CA /opt/kubernetes/ssl/ca.pem -CAkey /opt/kubernetes/ssl/ca-key.pem -CAcreateserial -out lemon-admin.crt -days 5000<br>
kubectl config set-credentials lemon-admin --client-certificate=lemon-admin.crt --client-key=lemon-admin.key --embed-certs=true<br>
kubectl config set-context lemon-admin@kubernetes --cluster=kubernetes --user=lemon-admin<br>
kubectl config use-context lemon-admin@kubernetes<br>

### 创建crd资源

kubectl apply -f  crd/school.yaml <br>
customresourcedefinition.apiextensions.k8s.io/classes.school.lemon.io created<br>

kubectl get  -f  crd/school.yaml  <br>
NAME                      CREATED AT <br>
classes.school.lemon.io   2021-01-10T18:43:32Z <br>

<br>

### 创建cr <br>
kubectl apply -f crd/class-one.yaml <br>
class.school.lemon.io/stu created <br>

kubectl get  -f crd/class-one.yaml  <br>
NAME   班级名称   班主任   学生人数   状态    AGE <br>
stu    高三一班   高某某   50           19s <br>

### cr伸缩
kubectl scale  cl stu --replicas=10
class.school.lemon.io/stu scaled <br>

kubectl get cl <br>
NAME   班级名称   班主任   学生人数   状态    AGE <br>
stu    高三一班   高某某   10           2m42s <br>

### 未解决问题
  状态管理未得到解决<br>







