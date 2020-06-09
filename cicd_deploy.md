# Jenkins 部分
## Jenkins 安装部署参考

https://phoenixnap.com/kb/how-to-install-jenkins-kubernetes

### Kuberbetes-- 利用Jenkins在Kubernetes中实践CI/CD
https://zhangchenchen.github.io/2017/12/17/achieve-cicd-in-kubernetes-with-jenkins/

### Jenkins k8s plugin
https://github.com/jenkinsci/kubernetes-plugin

### How To Install Jenkins On Kubernetes Cluster
https://phoenixnap.com/kb/how-to-install-jenkins-kubernetes

## key points

### 1、Jenkins master 要单独部署
https://www.jenkins.io/download/
在centos7上
https://linuxize.com/post/how-to-install-jenkins-on-centos-7/

### 2、为了提高CI的速度，支持大量用户同时构建，利用k8s 自动扩缩 Jenkins slave Pod 来进行。
这个需要[Jenkins k8s plugin](https://github.com/jenkinsci/kubernetes-plugin)插件来支持。 

---

# Set Up a CI/CD Pipeline with Kubernetes
https://github.com/zhangchenchen/kubernetes-ci-cd
https://github.com/mschmidt712/kubernetes-ci-cd
https://www.linux.com/audience/enterprise/set-cicd-pipeline-kubernetes-part-1-overview/

---

# 一些 devops 平台推荐

## 侧重于管理员或对K8S有一定使用经验的平台
https://rancher.com/
https://www.openshift.com/

## 侧重于开发人员
最新发布了 3.0 ，支持多集群
https://kubesphere.io/

## 猪齿鱼项目
Open Source Multi-Cloud Integrated Platform
http://choerodon.io/zh/docs/installation-configuration/

内部演示 admin/admin
http://ezdev-choerodon-front8080.saiciaas.shjq-uat-a.sxc.sh/

---

# 其他参考

## Top 10 open source platform to build your CI/CD stack
https://medium.com/cloudscaleqa/top-10-open-source-platform-to-build-your-ci-cd-stack-74a93a043d37

## google devops文档（只参考牛B的）
https://cloud.google.com/solutions/devops/devops-tech-version-control