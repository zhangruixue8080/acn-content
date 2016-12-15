<properties
                pageTitle="如何在不添加应用程序的前提下使用 PowerShell 获取 Azure Active Directory 令牌"
                description="使用 PowerShell 在不添加应用程序的前提下获取 Azure Active Directory 令牌"
                services="active-directory"
                documentationCenter=""
                authors=""
                manager=""
                editor=""
                tags="active-directory,Token,Azure 管理门户,PowerShell"/>

<tags
                ms.service="active-directory-aog"
                ms.date="12/15/2016"
                wacn.date="12/15/2016"/>

# 如何在不添加应用程序的前提下使用 PowerShell 获取 Azure Active Directory 令牌

众所周知当要获取 Azure Active Directory（ Azure AD ）安全令牌（ Token ）时，需要添加应用程序并且需要相应的 `clienID` 返回 URL 等信息来获取 Token，现在可以使用 PowerShell 脚本获取 Azure AD 的 Token 并且不需要在 Azure AD 中添加应用程序。

## 操作步骤：
1. 登录到 Azure 管理门户；
2. 在左侧的导航栏中单击 "Active Directory"；
3. 新建 Azure AD，如已有 Azure AD，则跳过该步骤；
4. 将以下代码保存为 .ps1 的后缀名，运行该脚本程序。  

**Powershell 代码：**  

```PowerShell
function GetAuthToken
{
       param
       (
              [Parameter(Mandatory=$true)]
              $TenantName
       )
 
       $adal = "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Services\Microsoft.IdentityModel.Clients.ActiveDirectory.dll"
 
       $adalforms = "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Services\Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll"
 
       [System.Reflection.Assembly]::LoadFrom($adal) | Out-Null
 
       [System.Reflection.Assembly]::LoadFrom($adalforms) | Out-Null
 
       $clientId = "1950a258-227b-4e31-a9cf-717495945fc2" 
 
       $redirectUri = "urn:ietf:wg:oauth:2.0:oob"
 
       $resourceAppIdURI = "https://graph.chinacloudapi.cn"
 
       $authority = "https://login.chinacloudapi.cn/$TenantName"
 
       $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList $authority
 
       $authResult = $authContext.AcquireToken($resourceAppIdURI, $clientId,$redirectUri, "Auto")
 
       return $authResult
}

$token = GetAuthToken -TenantName "xuhuadd.partner.onmschina.cn"  
$token 

```

**最后结果截图：**  

![result](./media/aog-active-directory-powershell-query-token/result.png)


