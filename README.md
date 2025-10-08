1.因為是試用Window Server 2022版，所以就將microsoft的連結丟上來  
2.到 [windows 2022](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022?msockid=21a32a8c5fc66b5b1dff3fdd5ea96a8f)  
3.選擇適用的下載  
4.下載完後如果不習慣windows只有terminal介面的請直接點第二個進行下載  
5.下載完後會跳出一個服務器  
<img width="680" height="657" alt="螢幕擷取畫面 2025-10-02 155157" src="https://github.com/user-attachments/assets/2d2e1fe8-a0ab-42bc-9ce4-19f9beba5fe4" />  
6.選擇服務器角色  
<img width="791" height="565" alt="螢幕擷取畫面 2025-10-02 155721" src="https://github.com/user-attachments/assets/61fb8281-f70b-4ff8-b20d-6cbe2ad24e68" />
將Active Direction Domain Server 與 DNS Server打勾並按下一步  
最後按下安裝就完成了  

接著下載[VS C++ 2013](https://www.microsoft.com/zh-TW/download/details.aspx?id=40784&msockid=3cc3b7a93710612c2941a267366d6000)

📁 一、邮箱数据库管理
功能	命令
查看所有数据库	Get-MailboxDatabase
挂载数据库	Mount-Database "DB01"
卸载数据库	Dismount-Database "DB01"
删除数据库	Remove-MailboxDatabase "DB01"
启用循环日志（节省空间）	Set-MailboxDatabase -Identity "DB01" -CircularLoggingEnabled $true
设置数据库邮箱大小限制	Set-MailboxDatabase -Identity "DB01" -IssueWarningQuota 1.5GB -ProhibitSendQuota 1.8GB -ProhibitSendReceiveQuota 2GB
👤 二、用户邮箱管理
功能	命令
创建新邮箱用户	New-Mailbox -Name "Alice" -UserPrincipalName alice@yourdomain.com -Password (ConvertTo-SecureString "P@ssw0rd" -AsPlainText -Force)
禁用邮箱（保留AD账号）	Disable-Mailbox -Identity "Alice"
删除邮箱和AD账号	Remove-Mailbox -Identity "Alice"
移动邮箱到其他数据库	New-MoveRequest -Identity "Alice" -TargetDatabase "DB02"
设置邮箱大小配额	Set-Mailbox -Identity "Alice" -ProhibitSendQuota 2GB
📬 三、通讯组与共享邮箱
功能	命令
创建通讯组	New-DistributionGroup -Name "Sales Team" -PrimarySmtpAddress sales@yourdomain.com
添加成员	Add-DistributionGroupMember -Identity "Sales Team" -Member alice@yourdomain.com
创建共享邮箱	New-Mailbox -Shared -Name "Support" -UserPrincipalName support@yourdomain.com
添加共享邮箱访问权限	Add-MailboxPermission -Identity "Support" -User "Alice" -AccessRights FullAccess
🌐 四、客户端访问配置（OWA/ActiveSync）
功能	命令
启用/禁用 OWA	Set-CASMailbox -Identity "Alice" -OWAEnabled $false
启用/禁用手机同步（ActiveSync）	Set-CASMailbox -Identity "Alice" -ActiveSyncEnabled $false
查看客户端设置	Get-CASMailbox -Identity "Alice"
📤 五、邮件流（Send / Receive Connectors）
功能	命令
查看所有接收连接器	Get-ReceiveConnector
创建发送连接器（出站邮件）	New-SendConnector -Name "Internet Out" -AddressSpaces "*" -Internet -DNSRoutingEnabled $true
限制 IP 发信	Set-ReceiveConnector -Identity "Default Frontend WIN-DR..." -RemoteIPRanges 192.168.1.0/24
🔐 六、安全与权限
功能	命令
委派邮箱访问权限	Add-MailboxPermission -Identity "Alice" -User "Bob" -AccessRights FullAccess
委派发送权限（Send As）	Add-ADPermission -Identity "Alice" -User "Bob" -ExtendedRights "Send As"
查看权限	Get-MailboxPermission -Identity "Alice"
🛠 七、管理工具与维护
功能	命令
测试邮箱服务（OWA/Autodiscover）	Test-OutlookWebServices -Identity "Alice"
查看邮箱使用量	Get-MailboxStatistics -Database "DB01"
备份状态（第三方备份软件配合）	需结合 VSS 或其他方案
更新 Exchange CU（累积更新）	手动下载、安装新版 CU
📊 八、报表与查询
功能	命令
导出所有邮箱列表	`Get-Mailbox
查询大邮箱	`Get-MailboxStatistics
查看邮箱登录信息	Get-LogonStatistics -Identity "Alice"
