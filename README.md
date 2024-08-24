![AutoSSL](https://socialify.git.ci/kimoch111/AutoSSL/image?description=1&descriptionEditable=%E4%B8%80%E4%B8%AA%E5%88%A9%E7%94%A8%20GitHub%20Actions%20%E5%92%8C%20acme.sh%20%E8%87%AA%E5%8A%A8%E5%8C%96%E7%AE%A1%E7%90%86%E5%9F%9F%E5%90%8D%20SSL%20%E8%AF%81%E4%B9%A6%E7%9A%84%E5%B0%8F%E9%A1%B9%E7%9B%AE&font=Inter&forks=1&issues=1&language=1&logo=https%3A%2F%2Favatars.githubusercontent.com%2Fu%2F133087009&name=1&owner=1&pattern=Floating%20Cogs&pulls=1&stargazers=1&theme=Light)

## :tada:AutoSSL

利用 GitHub Actions 以及 [acme.sh](https://github.com/acmesh-official/acme.sh)，实现了多域名自动申请 SSL 证书并保存，同时可对 SSL 证书自动续期。



## :rotating_light:Warning

> [!WARNING]
>
> **使用时，注意clone本仓库，并创建一个私有仓库来管理证书，防止证书的私钥泄露。**



## :white_check_mark:Features

- [x] 自动申请 SSL 证书，并保存证书到 SSL 目录下
- [x] 定期检查 SSL 证书，小于30天自动续期
- [x] 定期输出检查报告到 Certificate.md 文件
- [x] 证书是泛域名证书
- [x] 同时申请 ECDSA 和 RSA 证书
- [x] 支持多个不同 DNS 服务商下的多个域名
- [ ] 支持邮件&TG推送新证书



## :rocket:How to use

### 申请域名

略

### 申请 DNS 服务商 API Token

参考官方文档[dnsapi · acmesh-official/acme.sh Wiki (github.com)](https://github.com/acmesh-official/acme.sh/wiki/dnsapi)或其他文章

### 配置 Secret

在 GitHub 仓库中，依次访问 `Settings -> Security -> Secrets and variables -> Actions -> Repository secrets`，添加以下变量：

#### - DNSAPI

创建 DNSAPI 填写从 DNS 服务商获取的 API KEY，用到什么 DNS 解析就填什么，具体键名可参考官方文档[dnsapi · acmesh-official/acme.sh Wiki (github.com)](https://github.com/acmesh-official/acme.sh/wiki/dnsapi)对应中 DNS 服务商的 API

```bash
export DP_Id="xxxxxxxxxxxxxxxxxxxxxx"
export DP_Key="xxxxxxxxxxxxxxxxxxxxx"
export Tencent_SecretId="xxxxxxxxxxx"
export Tencent_SecretKey="xxxxxxxxxx"
export Ali_Key="xxxxxxxxxxxxxxxxxxxx"
export Ali_Secret="xxxxxxxxxxxxxxxxx"
export CF_Zone_ID="xxxxxxxxxxxxxxxxx"
export CF_Token="xxxxxxxxxxxxxxxxxxx"
export AWS_ACCESS_KEY_ID="xxxxxxxxxx"
export AWS_SECRET_ACCESS_KEY="xxxxxx"
```

#### - ACCOUNT_EMAIL

acme.sh 申请 SSL 需要的邮箱地址



### 设置 GitHub Actions 权限

在 GitHub 仓库中，依次访问 `Settings -> Code and automation -> Actions -> General -> Workflow permissions`，勾选 `Read and write permissions` 权限。

### 配置域名

修改`domains.yaml`文件，在其中对应 DNS 服务商下您的域名所在 DNS 解析进行修改，用到什么留什么，没用到的可以删了，每个 DNS 服务商下可以填写多个域名，将依次申请

```yaml
dnspod:
  - example.com
tencent:
  - example0.com
aliyun:
  - example1.com
  - example2.com
cloudflare:
  - example3.com
  - example4.com
aws:
  - example5.com
  - example6.com
```

默认已支持这5个DNS服务商，这时候你就要问了，如果您的域名所在DNS解析不在上述的说明中该怎么办？

你可以在`.github/workflows/AutoSSL.yaml`中修改这个地方，增加其他DNS服务商，例如：`["gadaddy"]="dns_gd"`,等号左边是你在`domains.yaml`中定义的键名，右边是acme的dns插件名

```yaml
        # Define the mapping of DNS providers to acme.sh plugins
        declare -A dns_plugins=(
          ["dnspod"]="dns_dp"
          ["aliyun"]="dns_ali"
          ["tencent"]="dns_tencent"
          ["cloudflare"]="dns_cf"
          ["aws"]="dns_aws"
          # Add other DNS providers and their corresponding acme.sh plugins here
          # 在此处添加其他DNS提供商及其相应的acme.sh插件
          ["gadaddy"]="dns_gd"
        )
```



#### 定时触发

工作流已配置每周一`UTC`时间`17`点，即北京时间凌晨`1`点自动运行


#### 手动触发

在 GitHub 仓库中，点击右上角的`Star`即可手动触发任务执行



## :children_crossing:Thanks

🙏感谢，在此项目基础上对代码改吧改吧

- https://github.com/danbao/auto-ssl
