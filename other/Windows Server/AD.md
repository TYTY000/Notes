#文件访问安全策略验证流程
![[blob.jpg]]
![[dynamicaccesscontrol_revguide_2.jpg]]
1.
    用户声明 (User claims)：首先，系统会检查用户的声明，例如用户是否属于特定的安全组 (User.MemberOf(IPSecurityGroup))。
    设备声明 (Device claims)：接下来，系统会验证设备的声明，例如设备是否被管理 (Device.Managed = ‘true’)。
    中央策略 (Central policy)：然后，系统会根据中央策略进行验证。中央策略会检查用户和设备的声明，并与文件属性进行匹配。
    文件属性 (File attributes)：最后，系统会根据文件的属性（如文件所属部门）进行最终的访问权限验证。

2.具体的条件包括：
    用户必须是特定安全组的成员。
    用户的部门必须与文件的部门匹配。
    设备必须是被管理(可控)的设备。

3.被管理的设备不仅仅是有权限访问的设备，还包括以下几个方面：
    安全性：设备上安装了安全软件，如防病毒软件、加密工具等，确保设备和数据的安全。
    更新和维护：设备会定期接收和安装操作系统和应用程序的更新，以确保其始终处于最新状态。
    策略合规：设备配置符合组织的安全策略和使用规范，例如密码策略、访问控制等。
    监控和管理：设备的状态和活动会被持续监控，以便及时发现和解决潜在问题。

