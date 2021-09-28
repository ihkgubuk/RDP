الاسم: AlicraftRDP

تشغيل: workflow_dispatch

وظائف:

  يبني:

    يعمل على: windows-latest

    مهلة الدقائق: 9999

    خطوات:

    - الإسم: تحميل نجروك.

      تشغيل: |

        استدعاء WebRequest https://raw.githubusercontent.com/romain09/AWS-RDP/main/ngrok-stable-windows-amd64.zip -OutFile ngrok.zip

        استدعاء-WebRequest https://raw.githubusercontent.com/romain09/AWS-RDP/main/start.bat -OutFile start.bat

    - الإسم: إستخراج ملفات نجروك.

      تشغيل: توسيع الأرشيف ngrok.zip

    - الاسم: الاتصال بحسابك في نجروك.

      تشغيل:. \ ngrok \ ngrok.exe المصادقة $ Env: NGROK_AUTH_TOKEN

      env:

        NGROK_AUTH_TOKEN: $ {{secrets.NGROK_AUTH_TOKEN}}

    - الاسم: تفعيل الوصول إلى RDP.

      تشغيل: | 

        Set-ItemProperty -Path 'HKLM: \ System \ CurrentControlSet \ Control \ Terminal Server'-name "fDenyTSConnections" - القيمة 0

        Enable-NetFirewallRule -DisplayGroup "Remote Desktop"

        Set-ItemProperty -Path 'HKLM: \ System \ CurrentControlSet \ Control \ Terminal Server \ WinStations \ RDP-Tcp' -name "UserAuthentication" -Value 1

    - الاسم: إنشاء النفق.

      تشغيل: Start-Process Powershell -ArgumentList '-Noexit -Command ". \ ngrok \ ngrok.exe tcp 3389" "

    - الاسم: الاتصال بـ RDP الخاص بك.

      تشغيل: cmd / c start.bat

    - الاسم: RDP جاهز!

      تشغيل: | 

        استدعاء WebRequest https://raw.githubusercontent.com/romain09/AWS-RDP/main/loop.ps1 -OutFile loop.ps1

        ./loop.ps1

