# eks-cluster-with-Fargate
EKS on Fargate (ALB Ingress)
![image](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/c5c48829-c40a-4dcc-8ba5-f153d2c99590)

아래 명령어로 CloudFormation Stack생성
```
$ aws cloudformation create-stack --stack-name eks-vpc --template-url "https://samsung-s3-bucket.s3.us-west-2.amazonaws.com/cf-code/eks-vpc.yml" --region us-east-2
```
![image](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/22b1e13e-4df8-4ea9-ba7f-401a33d13988)
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/be5e82f2-1826-46d1-b883-ec47e69f0c17)
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/90844429-4504-41c1-bd21-3e02889e93d8)
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/bdb1c7cc-fc4c-4261-8603-20840b85fa8f)
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/bbbe421d-6d28-4965-a444-43b8b50000fc)
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/367703f6-654a-46f9-82fa-b604a24a1222)
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/cabdd560-f9bb-4afc-a91e-4d48e3b234ba)
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/29efa0d3-077c-4f74-b3a6-fe017897628d)
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/5b4edcd4-4654-40fb-bdd4-17651fa66ae9)
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/9efa7073-b40d-4acd-bc07-76231f9ca99f)
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/4a12b505-d54d-4d74-a932-46bf44ca9e34)
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/456b9672-b5a5-46a3-95c1-0f682099dbea)
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/c7c80c9d-65ac-448a-a26e-c88625ef312a)
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": "eks:DescribeCluster",
            "Resource": "*"
        }
    ]
}
```
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/83e68279-bd25-4f81-9e64-b156b0a2071d)
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/5e2dc1ec-610c-4595-9286-0a3ad904891f)
해당 Role을 생성하고 IAM Instance Profile에 이전에 생성한 IAM Role을 선택
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/ea998480-3aef-4bcf-81fe-2cee487408eb)
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/b75fdb0a-08e9-45c3-afb1-ce8991f40979)
```
#!/bin/bash
sed -i 's|PasswordAuthentication no|PasswordAuthentication yes|g' /etc/ssh/sshd_config
echo '<Password>' | passwd --stdin ec2-user
systemctl restart sshd
```
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/324d1122-a039-49df-87c3-9ed03392a3c0)
해당 Instance가 정상 적으로 생성이 되었는지 확인
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/25f9710d-e21a-4a3e-bd3d-1748de6af8f4)
### Connection to EKS Instance
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/3bf2e73e-57c9-45f0-9844-2b8bdcd60abf)
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/7cd348ac-6785-4f9a-b087-c5097b168b46)
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/f0b65741-3a6b-4290-b1b4-8fa89a141216)
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/034f3050-cc05-4504-b71c-42869c1af26f)
kubectl 설치
```
curl -o kubectl https://s3.us-west-2.amazonaws.com/amazon-eks/1.23.7/2022-06-29/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
```
eksctl 설치
```
ARCH=amd64
PLATFORM=$(uname -s)_$ARCH
curl -sLO "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$PLATFORM.tar.gz"
curl -sL "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_checksums.txt" | grep $PLATFORM | sha256sum --check
tar -xzf eksctl_$PLATFORM.tar.gz -C /tmp && rm eksctl_$PLATFORM.tar.gz
sudo mv /tmp/eksctl /usr/local/bin
```
Helm 설치
```
$ curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 > get_helm.sh
$ chmod 700 get_helm.sh
$ ./get_helm.sh
```
### Configure AWS Configure
EKS에는 생성한 사용자 즉, ROOT 계정으로 EKS Cluster를 구축했으면 ROOT 사용자만 접근 가능

![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/14ade18c-f4f7-4fb4-a833-a77b2f14e850)

이전 생성한 Instance에 aws configure를 사용해서 root 사용자를 로그인
```
$ aws configure
```
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/75339488-931d-47eb-81f5-15ad397e58b7)
### EKS Cluster
EKS Cluster Service Role을 생성

![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/7638acf2-0ad5-4570-ada3-189ba5b48ebc)
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/ec3ee814-9aa0-4aa8-a7e3-2b8ff70673d6)
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/1cc8a969-4556-4094-99bd-2fb6e38e1d00)
해당 Role과 Policy를 생성

![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/d09b9742-93ab-4758-b268-d88f09c06620)
해당 IAM Role의 ARN 주소를 복사

![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/d61e5560-40d7-4e82-8074-0bc4bd1e8644)
Instance로 접근하고 아래와 같은 명령어를 사용해서 EKS Cluster를 생성

```
$ vim cluster-config.yaml
```
```
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: eks-cluster\
  region: us-east-2

vpc:
  subnets:
    private:
      us-east-2a: { id: subnet-076137945cd7acb36 }
      us-east-2b: { id: subnet-028968e58b0b8bdb8 }

fargateProfiles:
  - name: flask-fargate-profile
    selectors:
      - namespace: flaskapp
  - name: kube-system-profile
    selectors:
      - namespace: kube-system
```
해당 yaml을 적용시켜서 EKS Cluster를 생성

```
$ eksctl create cluster -f cluster-config.yaml
```
### Connect to eks cluster
아래의 명령어를 통해서 ~/.kube/config 파일을 업데이트

```
$ aws eks update-kubeconfig --name eks-cluster
```
~/.kube/config 파일을 열어보면 아래와 같이 내용이 업데이트되어 있다

![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/0c079ea1-36cd-4c28-bb26-eebd0e818158)
certificate-authority-data 항목에 인증서에 대한 정보가 쓰여져 있는 것을 볼 수 있다. 위와 같이 EKS는 인증서를 사용해서 EKS Cluster에 인증을 하는 것을 알 수 있다.

![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/c1d5796d-da3b-4b77-97e7-11dea114ce51)
server: https//*eks.amazonaws.com 라는 도메인이 있는데 이는 EKS Cluster의 Endpoint주소이다.

### Deploy Web Application
flaskapp 이라는 namspace를 생성

```
$ kubectl create ns flaskapp
```
flask로 개발된 sample application을 fargate를 통해서 배포

```
apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: flaskapp
  name: deployment-flask
spec:
  selector:
    matchLabels:
      app.kubernetes.io/name: app-flask
  replicas: 5
  template:
    metadata:
      labels:
        app.kubernetes.io/name: app-flask
    spec:
      containers:
      - image: ghcr.io/dispiny/flask:logging-app
        imagePullPolicy: Always
        name: app-flask
        ports:
        - containerPort: 80
```
```
$ kubectl apply -n flaskapp -f flask-deployment.yaml
```
### Install AWS Load Balancer Ingress
Cluster가 Service Account에서 AWS IAM을 사용할 수 있도록 허용

```
$ eksctl utils associate-iam-oidc-provider --cluster=eks-cluster --approve
```
AWS Load Balancer Controller에서 사용자 대신 AWS API 호출하는 것을 허용하는 IAM정책을 다운로드

```
$ curl -o iam_policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.2.0/docs/install/iam_policy.json
```
다운로드한 정책을 사용해 IAM 정책을 생성

```
$ aws iam create-policy \
   --policy-name AWSLoadBalancerControllerIAMPolicy \
   --policy-document file://iam_policy.json
```
AWS Load Balancer Controller에 대한 kube-system 네임스페이스에 aws-load-balancer-contoller라는 이름의 서비스 계정을 생성

```
$ eksctl create iamserviceaccount \
  --cluster=eks-cluster\
  --namespace=kube-system \
  --name=aws-load-balancer-controller \
  --attach-policy-arn=arn:aws:iam::948216186415:policy/AWSLoadBalancerControllerIAMPolicy \
  --override-existing-serviceaccounts \
  --approve
```
helm에 Amazon EKS Chart Repository를 추가

```
$ helm repo add eks https://aws.github.io/eks-charts
```
TargetGroupBinding 사용자 지정 리소스 정의(CRD)를 설치

```
$ kubectl apply -k "github.com/aws/eks-charts/stable/aws-load-balancer-controller//crds?ref=master"
```
Helm 차트를 설치

```
$ helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
    --set clusterName=eks-cluster \
    --set serviceAccount.create=false \
    --set serviceAccount.name=aws-load-balancer-controller \
    -n kube-system \
    --set region="us-east-2" \
    --set vpcId="vpc-09069ffef05a68d26" 
```
Public Subnet에 아래와 같은 Tag를 추가

![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/3ceaee8a-5f8b-4fc8-9cf2-32a28a02468c)
### Deploy Flask App & ALB
service를 생성

```
$ vim flask-svc.yaml
```
```
apiVersion: v1
kind: Service
metadata:
  namespace: flaskapp
  name: service-flask
spec:
  ports:
    - port: 80
      targetPort: 80
      protocol: TCP
  type: NodePort
  selector:
    app.kubernetes.io/name: app-flask
```
Ingress를 생성

```
$ vim flask-ing.yaml
```
```
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  namespace: flaskapp
  name: ingress-flask
  annotations:
    kubernetes.io/ingress.class: alb
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/target-type: ip
spec:
  rules:
    - http:
        paths:
          - path: /*
            backend:
              serviceName: service-flask
              servicePort: 80
```
정의한 메니패스트 파일을 모두 적용

```
$ kubectl apply -f flask-ing.yaml -f flask-svc.yaml -n flaskapp
```
정상 적으로 모두 생성이 되었는지 확인

```
$ kubectl get pods -n flaskapp
```
![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/e0b74ce3-57c3-49f9-b0ab-2b8cf998ea8f)
해당 ALB DNS 주소로 접근 했을 때 정상 적으로 접근이 되는지 확인.

![Untitled](https://github.com/LeeSeokBln/eks-cluster-with-Fargate/assets/101256150/1d12a32a-4f4e-498a-b122-5bc234718074)
