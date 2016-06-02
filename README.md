# deployGitlab
Deploy your Gitlab

# 安装 Gitlab
-----------------------
安装邮件服务器

	sudo apt-get update
		
下载和安装

	wget https://downloads-packages.s3.amazonaws.com/ubuntu-14.04/gitlab-ce_7.10.0~omnibus.2-1_amd64.deb
	sudo dpkg -i gitlab-ce_7.10.0~omnibus.2-1_amd64.deb
	
修改配置

	sudo vim /etc/gitlab/gitlab.rb
	
*主要修改 external_url、email 和 ldap 相关内容*

```
external_url 'http://gitlab.yintai.org'
gitlab_rails['gitlab_email_enabled'] = true
gitlab_rails['gitlab_email_from'] = 'project-report@yintai.com'
gitlab_rails['gitlab_email_display_name'] = 'gitlab'
gitlab_rails['gitlab_email_reply_to'] = 'noreply@yintai.com'

gitlab_rails['smtp_enable'] = true
gitlab_rails['smtp_address'] = "smtp.yintai.com"
gitlab_rails['smtp_port'] = 587
gitlab_rails['smtp_user_name'] = "project-report"
gitlab_rails['smtp_password'] = "ChangeMe"
gitlab_rails['smtp_domain'] = "yintai.com"
gitlab_rails['smtp_authentication'] = "login"
gitlab_rails['smtp_enable_starttls_auto'] = true

gitlab_rails['ldap_enabled'] = true
gitlab_rails['ldap_servers'] = YAML.load <<-EOS
main:
  label: 'LDAP'
  host: 'yt.corp'
  port: 389
  uid: 'sAMAccountName'
  allow_username_or_email_login: false
  method: 'plain'
  bind_dn: 'CN=ADAuth,OU=Special Accounts,OU=Beijing,OU=YinTai,DC=yt,DC=corp'
  password: 'ChangeMe'
  block_auto_created_users: false
  active_directory: true
  base: 'OU=YinTai,DC=yt,DC=corp'
  user_filter: '(&(objectCategory=Person)(sAMAccountName=*))'
EOS
```

使配置生效

	sudo gitlab-ctl reconfigure
	
默认管理员密码

	Username: root 
	Password: 5iveL!fe
