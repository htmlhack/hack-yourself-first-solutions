# Hack Youself First 解决方案

注意事项：

此处提供的解决方案仅供教育用途，不是为了提倡不道德的黑客行为。

在 YouTube 订阅我：https://www.youtube.com/channel/UCzs_T0LXCaIfr8hLdpwg7gg

# 01 未保护的敏感页面 / 信息泄露

摘要：未受保护的敏感页面可能使攻击者查看到敏感信息，如用户名、电子邮件、密码等。

复现步骤：复现该漏洞只有一个步骤，访问 "/api/admin/users"（https://hack-yourself-first.com/api/admin/users）。

影响：该漏洞允许攻击者查看所有用户的敏感信息，可能导致所有用户的账户被攻破。

# 02 "密码已被使用"

摘要：这是一个非常有趣的漏洞，允许攻击者完全接管账户。

复现步骤：使用较不常见的密码（例如：123）创建一个账户，系统会提示“密码已被使用”。通过这个提示，我们可以得知电子邮件和密码，并登录到账户。

影响：该漏洞允许攻击者窃取许多用户的密码，并可能导致账户被完全接管。

# 03 注销后 Cookie 无效

摘要：用户从应用程序注销时，通常认为没有人能访问他们的账户，但 cookie 无效问题破坏了这一点。

复现步骤：在两个浏览器中登录您的账户，在其中一个浏览器中注销，然后在第二个浏览器中重新加载页面，您会发现该浏览器中的会话仍然有效。

影响：即使用户从其账户注销，攻击者仍然可以使用相同的会话 cookie 登录。

# 04 修改密码缺乏安全性

摘要：修改密码时缺乏安全性，当受害者的会话被劫持且攻击者已经登录时，会导致账户被完全接管。

复现步骤：进入更改密码页面，输入任何随机文本并点击更改密码，将在没有验证的情况下被更改。

影响：该漏洞允许攻击者在劫持受害者的会话后接管整个账户。

# 05 修改密码后 Cookie 无效

摘要：用户更改密码时，旧的会话没有失效，这导致了一个漏洞。

复现步骤：在两个浏览器中登录您的账户，在一个浏览器中更改密码，然后在第二个浏览器中重新加载页面，您会发现该浏览器中的会话仍然有效。

影响：由于此漏洞，如果账户已经被攻破，受害者没有办法撤销攻击者的访问权限。

# 06 编辑资料中的 IDOR 漏洞

摘要：IDOR 漏洞通常使攻击者能够查看、编辑甚至删除应用程序中的任何随机用户的敏感信息。

复现步骤：登录您的账户，进入编辑资料页面，并查看浏览器中的链接。链接格式为 "https://hack-yourself-first.com/Account/UserProfile/... "，您可以看到自己的用户 ID，通过修改它，您可以访问并编辑其他用户的资料。

影响：由于此漏洞，攻击者可以查看和更改应用程序中每个用户的用户名。

# 07 创建账户缺乏验证

摘要：在创建账户时缺乏验证，导致攻击者滥用受害者的电子邮件。

复现步骤：使用任何电子邮件创建一个账户，您会发现没有验证过程来确认该电子邮件是否属于所称的人。

影响：由于此漏洞，攻击者可以滥用电子邮件，给受害者带来不良影响。

# 08 批量创建账户

摘要：在创建账户时缺乏验证，导致攻击者滥用受害者的电子邮件，并批量创建多个受害者的账户。

复现步骤：创建一个电子邮件和密码列表，进入创建账户页面。输入任意随机值并在 Burp 中拦截请求。将请求发送到 Intruder，清除默认位置，添加 "Email"、"Password" 和 "ConfirmPassword" 的值，并将攻击类型更改为 pitchfork。在 Payloads 中，粘贴电子邮件列表到 Payload set 1，密码列表到 Payload set 2 和 Payload set 3，然后开始攻击。现在列表中的所有电子邮件都已创建为账户。

影响：由于此漏洞，攻击者可以在服务器上制造大量垃圾账户，如果电子邮件是真实的，那么攻击者就滥用了这些电子邮件。

# 09 通过 Cookie 提权

摘要：权限提升授予未授权用户访问页面的权限。

复现步骤：复现此问题有两步，我将它们列举如下：第一步，重新加载页面并拦截请求，将 "isAdmin" 的 Cookie 值从 false 改为 true，然后转发。您会看到 "Admin" 在 "我的账户" 附近，点击它会将您带到用户列表页面，包括他们的电子邮件和密码。第二步是直接浏览 "/Secret/Users/"（https://hack-yourself-first.com/Secret/Users），它会将您带到包含用户电子邮件和密码的相同页面。

影响：由于此漏洞，攻击者可以查看和编辑所有用户的详细信息。

# 10 电子邮件伪造

摘要：缺少 SPF 记录导致电子邮件伪造。

复现步骤：访问 "https://www.kitterman.com/spf/validate.html/"。然后输入域名（hack-yourself-first.com）并点击 "Get SPF Record (if any)"，您会看到 "No valid SPF record found."。现在您可以使用任何电子邮件伪造工具向受害者发送伪造的电子邮件。

影响：由于此漏洞，攻击者可以向受害者发送虚假邮件，重定向他们到钓鱼页面并窃取凭据。

# 11 速率限制

摘要：缺乏速率限制导致暴力破解密码。

复现步骤：进入登录页面，输入一个有效的电子邮件地址并在密码字段中输入随机值。在 Burp 中捕获请求并发送到 Intruder，清除默认位置并添加密码字段的值。创建一个假密码列表，最后输入原始密码。在 Payloads 中粘贴密码列表并开始攻击。原始密码将返回 302 响应，长度较小。

影响：由于此漏洞，攻击者可以通过暴力破解密码访问受害者的账户。

# 12 使用 IDOR 批量修改资料

摘要：IDOR 不仅能查看或修改特定用户，还可以用来修改所有用户的资料。

复现步骤：进入编辑资料页面并输入您想要的用户名。点击保存并在 Burp 中捕获请求，然后发送到 Intruder。在 Intruder 中添加 UserId 的值，并将 Payload 类型更改为 Numbers。在 Payload 选项中，From 设为 1，To 设为 500（例如），Step 设为 1，然后开始攻击。攻击结束时，您可以看到所有存在的用户 ID 返回 302 响应，而不存在的 ID 返回 500 响应，并且您还可以看到所有用户名都已更改。

影响：由于此漏洞，攻击者可以批量编辑用户资料，导致声誉受损。

# 13 没有最低密码要求

摘要：在创建账户时，并没有规定密码必须包含最低字符数。

复现步骤：创建一个账户并输入一个位数密码，您会看到该密码被接受。

影响：由于此漏洞，攻击者可以轻松暴力破解密码。

# 14 重置密码锁定合法用户

摘要：在忘记密码页面，要求输入电子邮件地址并点击重置，然后系统表示密码已重置并会将新密码发送到电子邮件中。

复现步骤：点击登录，然后点击“忘记密码”，输入电子邮件并点击重置。

影响：由于此漏洞，攻击者可以在用户不知情的情况下重置合法用户的密码，这可能导致账户被永久锁定。

# 15 重置所有用户的密码

摘要：在忘记密码页面，要求输入电子邮件地址并点击重置，然后系统表示密码已重置并会将新密码发送到电子邮件中。

复现步骤：创建一个使用该网站的用户列表，点击登录，然后点击“忘记密码”，输入电子邮件并点击重置并在 Burp 中捕获请求。将请求发送到 Intruder 并添加 Email 字段的值。现在粘贴应用程序用户列表并开始攻击。

影响：由于此漏洞，攻击者可以在用户不知情的情况下重置所有合法用户的密码，因为大多数用户使用假电子邮件创建账户，可能导致账户永久锁定。

# 16 登录 CSRF

摘要：登录页面中的 CSRF（跨站请求伪造）使受害者登录到攻击者的账户。

复现步骤：进入登录页面并输入您的电子邮件和密码。点击登录并在 Burp 中捕获请求。然后右键点击请求，点击 Engagement Tools 并生成 CSRF PoC。在选项中启用“包含自动提交脚本”，点击重新生成并复制代码，放弃登录请求。现在在您的系统中创建一个 HTML 文件，粘贴复制的 CSRF PoC 并保存并运行该文件。现在您可以看到您已登录到您的账户。

影响：这不是应用程序中的大问题，因为它没有任何敏感操作。实际应用中，如果受害者更改密码，攻击者可以查看密码，如果受害者在

该网站上购买商品，攻击者也可以看到购买记录。

如果您没有 Burpsuite Pro，以下是 CSRF PoC:

```html
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="https://hack-yourself-first.com/Account/Login" method="POST">
      <input type="hidden" name="Email" value="$email&#64;$email&#46;$email" />
      <input type="hidden" name="Password" value="$password" />
      <input type="hidden" name="RememberMe" value="false" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      document.forms[0].submit();
    </script>
  </body>
</html>
```

更新 $email 和 $password 为相应的值，

"&#64" 是 "@" 符号
"&#46" 是 "." 符号

例如，如果您的电子邮件是 "example@hacker.com"，那么您的电子邮件将显示为 "example&#64;hacker&#46;com"，
但它将被解码为 "example@hacker.com"。

# 17 注销 CSRF

摘要：注销页面中的 CSRF（跨站请求伪造）使受害者从其账户注销。

复现步骤：点击 Log Off 并在 Burp 中捕获请求。然后右键点击请求，点击 Engagement Tools 并生成 CSRF PoC。在选项中启用“包含自动提交脚本”，点击重新生成并复制代码，放弃请求。现在在您的系统中创建一个 HTML 文件，粘贴复制的 CSRF PoC 并保存并运行该文件。现在您可以看到您已从您的账户注销。

影响：攻击者在使用 #14步 重置密码后，可以使用户注销，导致受害者的账户被永久锁定。

如果您没有 Burpsuite Pro，以下是 CSRF PoC:

```html
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="https://hack-yourself-first.com/Account/LogOff" method="POST">
      <input type="submit" value="Submit request" />
    </form>
    <script>
      document.forms[0].submit();
    </script>
  </body>
</html>
```

# 18 使用 CSRF 更改密码

摘要：CSRF（跨站请求伪造）使得攻击者能够更改受害者的密码。

复现步骤：进入更改密码页面，输入任意文本并在 Burp 中捕获请求。然后右键点击请求，选择“engagement tools”，生成 CSRF PoC。在选项中启用自动提交脚本，点击重新生成并复制代码，丢弃请求。现在，在你的系统中创建一个 HTML 文件，粘贴复制的 CSRF PoC 并保存，运行该文件。现在你可以看到你的密码已经被更改。

影响：攻击者可以重置受害者的密码，从而完全控制账户。

如果你没有 Burpsuite pro，这里是 CSRF PoC：

```html
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="https://hack-yourself-first.com/Account/ChangePassword" method="POST">
      <input type="hidden" name="NewPassword" value="$newpassword" />
      <input type="hidden" name="ConfirmPassword" value="$newpassword" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      document.forms[0].submit();
    </script>
  </body>
</html>
```

将 `$newpassword` 更新为你希望更改的密码。

# 19 使用 CSRF 编辑个人资料

摘要：CSRF（跨站请求伪造）使攻击者能够更改受害者账户的姓名。

复现步骤：进入编辑个人资料页面，输入任意文本并在 Burp 中捕获请求。然后右键点击请求，选择“engagement tools”，生成 CSRF PoC。在选项中启用自动提交脚本，点击重新生成并复制代码，丢弃请求。现在，在你的系统中创建一个 HTML 文件，粘贴复制的 CSRF PoC 并保存，运行该文件。现在你可以看到你的名字和姓氏已经被更改。

影响：攻击者可以编辑受害者的个人资料。

如果你没有 Burpsuite pro，这里是 CSRF PoC：

```html
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="https://hack-yourself-first.com/Account/UserProfile/$userid" method="POST">
      <input type="hidden" name="IsAdmin" value="" />
      <input type="hidden" name="UserId" value="333" />
      <input type="hidden" name="FirstName" value="$firstname" />
      <input type="hidden" name="LastName" value="$lastname" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      document.forms[0].submit();
    </script>
  </body>
</html>
```

将 `$userid`, `$firstname` 和 `$lastname` 更新为相应的值。

# 20 使用 CSRF 重置密码

摘要：CSRF（跨站请求伪造）使得攻击者能够重置受害者的密码。

复现步骤：创建一个账户并登出。进入登录页面，转到忘记密码，输入电子邮件并点击重置。现在在 Burp 中捕获请求。然后右键点击请求，选择“engagement tools”，生成 CSRF PoC。在选项中启用自动提交脚本，点击重新生成并复制代码，丢弃请求。现在，在你的系统中创建一个 HTML 文件，粘贴复制的 CSRF PoC 并保存，运行该文件。现在你可以看到系统提示“你的密码已被重置，你将很快通过电子邮件收到一个新密码。”这与 #14 和 #15 类似，但攻击方法不同。

影响：由于这个漏洞，攻击者可以在合法用户不知情的情况下重置密码，而且大多数用户使用假电子邮件创建账户，这可能导致永久的账户锁定。

如果你没有 Burps

uite pro，这里是 CSRF PoC：

```html
<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="https://hack-yourself-first.com/Account/ResetPassword" method="POST">
      <input type="hidden" name="Email" value="email&#64;email&#46;$email" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      document.forms[0].submit();
    </script>
  </body>
</html>
```

将 `$email` 更新为相应的值，例如，如果你的电子邮件是 "example@hacker.com"，则你的电子邮件将显示为 "example&#64;hacker&#46;com"，但它将被解码为 "example@hacker.com"。

# 21 使用 Clickjacking 编辑个人资料

摘要：利用点击劫持漏洞来更改用户的个人资料。

复现步骤：创建一个账户并进入编辑个人资料页面。然后创建以下 HTML 文件（点击劫持 PoC）并运行该文件。接着输入下面的详细信息并重新加载 hack-yourself-first 网站的编辑个人资料页面。你可以看到你之前在创建的 HTML 文件中输入的内容。

影响：由于这个漏洞，攻击者可以更改受害者的个人资料，受害者会相信这些更改是他们自己做的。

点击劫持 PoC:

```html
<!DOCTYPE html>
<html>
<head>
<title>Clickjacking to change username</title>
</head>

<body>

<style>
    iframe {
        position:relative;
        width:700px;
        height: 500px;
        opacity: 0.00001;
        z-index: 2;
    }
    .ok {
        position:absolute;
        top:310px;
        left:220px;
        z-index: 1;
    }
    
    .animal { 
        position:absolute;
        top:210px;
        left:30px;
        z-index: 1;
    }
    
       .bird { 
        position:absolute;
        top:260px;
        left:50px;
        z-index: 1;
    }
    
    .input1 { 
        position:absolute;
        top:205px;
        left:210px;
        z-index: 1;
    }
    
      .input2 { 
        position:absolute;
        top:255px;
        left:210px;
        z-index: 1;
    }
    
    .delete { 
        position:absolute;
        top: 20px;
        left:50px;
        z-index: 1;
    }
    
</style>
<div class="animal">Enter an animal name:</div>
<div class="bird">Enter a bird name:</div>
<div class="ok">Ok</div>
<h4 class="delete">Press "Ctrl+A" and "Del" before typing</h4>
<input type="text" class="input1"></input>
<input type="text" class="input2"></input>
<iframe src="https://hack-yourself-first.com/Account/UserProfile/333"></iframe>

</body>
</html>
```

注意：这在 Firefox 中可以无问题工作，如果你使用其他浏览器，则必须增加 iframe 的透明度并相应调整。

# 22 无双因素认证

摘要：双因素认证（2FA）提供了额外的安全层，但该应用程序未能实现。

影响：由于此缺陷，用户相信他们的密码不会被破解，从而使他们的账户处于风险中。

# 23 SQL 注入

摘要：SQL 注入可能使攻击者查看表中所有内容。

复现步骤：访问以下地址："https://hack-yourself-first.com/CarsByCylinders?Cylinders=V12" ，你将在页面中看到一些内容。然后，重新加载页面并在 Burp 中捕获请求，修改请求为 `"GET /CarsByCylinders?Cylinders=V12'+OR+1=1-- - HTTP/2"` ，继续发送请求。现在你会看到额外的内容。

影响：由于此漏洞，攻击者可以查看应用程序不希望公开的额外信息。
