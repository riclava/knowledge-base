# Windows 开发相关

## Inno Setup 打包工具

### 安装前执行命令

```pascal
[Code]
function InitializeSetup(): boolean;
var
  ResultCode: integer;
begin
  // 启动程序并等待其结束
  if Exec(ExpandConstant('{win}\notepad.exe'), '', SW_SHOW, ewWaitUntilTerminated, ResultCode) then
  begin
    // 成功时处理，ResultCode 包含退出码
  end
  else begin
    // 失败时处理，ResultCode 包含错误码
  end;

  // 继续安装
  Result := True;
end;
```

### 完整示例

```ini
; Inno Setup 脚本示例

#define MyAppName "统一浏览器"
#define MyAppNameEn "MolanUpdaterService"
#define MyAppVersion "84.0.4147.146"
#define MyAppPublisher "CQUPT, Edu."
#define MyAppURL "https://www.cqupt.edu.cn/"
#define MyAppExeName "molan-updater-service.exe"
#define MolanInstaller "mini_installer.exe" 

[Setup]
AppId={{B3CA9D9D-D035-4398-8A0B-D85FE98F9C42}
AppName={#MyAppName}
AppVersion={#MyAppVersion}
AppPublisher={#MyAppPublisher}
AppPublisherURL={#MyAppURL}
AppSupportURL={#MyAppURL}
AppUpdatesURL={#MyAppURL}
DefaultDirName={autopf}\{#MyAppNameEn}
DisableProgramGroupPage=yes
OutputBaseFilename={#MyAppName}-{#MyAppVersion}
Compression=lzma
SolidCompression=yes
WizardStyle=modern

[Languages]
Name: "english"; MessagesFile: "compiler:Default.isl"; LicenseFile: "..\license.txt"
Name: "chinesesimplified"; MessagesFile: "compiler:Languages\ChineseSimplified.isl"; LicenseFile: "..\license.txt"

[Files]
Source: "..\{#MyAppExeName}"; DestDir: "{app}"; Flags: ignoreversion
Source: "{#GetEnv('MOLAN_RELEASE_DIR')}\{#MolanInstaller}"; DestDir: "{app}"; Flags: ignoreversion

[Icons]
Name: "{autoprograms}\{#MyAppName}"; Filename: "{app}\{#MyAppExeName}"

[Run]
Filename: {app}\{#MolanInstaller}; Parameters: ""; Flags: runhidden
Filename: {app}\{#MyAppExeName}; Parameters: "-service=install"; Flags: runhidden
Filename: {app}\{#MyAppExeName}; Parameters: "-service=start"; Flags: runhidden

[UninstallRun]
Filename: {app}\{#MyAppExeName}; Parameters: "-operation=uninstall"; Flags: runhidden
Filename: {app}\{#MyAppExeName}; Parameters: "-service=stop"; Flags: runhidden
Filename: {app}\{#MyAppExeName}; Parameters: "-service=uninstall"; Flags: runhidden
```

---

## 生成机器码

通过 WMI 获取硬件信息生成唯一机器码，支持 MFC 和 Win32 两种实现。

### 可获取的硬件信息

| 信息 | WMI 类 | 属性 |
|------|--------|------|
| BIOS UUID | Win32_ComputerSystemProduct | UUID |
| 处理器 ID | Win32_Processor | ProcessorId |
| 主板序列号 | Win32_BaseBoard | SerialNumber |
| 主板型号 | Win32_BaseBoard | Product |
| 主板制造商 | Win32_BaseBoard | Manufacturer |
| BIOS 序列号 | Win32_BIOS | SerialNumber |
| 硬盘序列号 | Win32_DiskDrive | SerialNumber |

### MFC 版本

#### WmiInfo.h

```cpp
#pragma once  
#include <afxpriv.h>
#include <WbemIdl.h>  
#pragma comment(lib, "WbemUuid.lib")  

class CWmiInfo
{
public:
  CWmiInfo(void);
  ~CWmiInfo(void);

  HRESULT InitWmi();
  HRESULT ReleaseWmi();

  BOOL GetSingleItemInfo(CString, CString, CString&);
  BOOL GetGroupItemInfo(CString, CString[], int, CString&);

private:
  void VariantToString(const LPVARIANT, CString&) const;

private:
  IEnumWbemClassObject* m_pEnumClsObj;
  IWbemClassObject* m_pWbemClsObj;
  IWbemServices* m_pWbemSvc;
  IWbemLocator* m_pWbemLoc;
};
```

#### WmiInfo.cpp

```cpp
#include "WmiInfo.h"  

CWmiInfo::CWmiInfo(void)
{
  m_pWbemSvc = NULL;
  m_pWbemLoc = NULL;
  m_pEnumClsObj = NULL;
}

CWmiInfo::~CWmiInfo(void)
{
  m_pWbemSvc = NULL;
  m_pWbemLoc = NULL;
  m_pEnumClsObj = NULL;
}

HRESULT CWmiInfo::InitWmi()
{
  HRESULT hr;

  // 初始化 COM 组件
  hr = ::CoInitializeEx(0, COINIT_MULTITHREADED);
  if (SUCCEEDED(hr) || RPC_E_CHANGED_MODE == hr)
  {
    // 设置进程安全级别
    hr = CoInitializeSecurity(NULL, -1, NULL, NULL,
      RPC_C_AUTHN_LEVEL_PKT, RPC_C_IMP_LEVEL_IMPERSONATE,
      NULL, EOAC_NONE, NULL);

    // 创建 WMI 命名空间连接
    hr = CoCreateInstance(CLSID_WbemLocator, 0, CLSCTX_INPROC_SERVER,
      IID_IWbemLocator, (LPVOID*)&m_pWbemLoc);
    VERIFY(SUCCEEDED(hr));

    // 连接到 root\cimv2
    hr = m_pWbemLoc->ConnectServer(CComBSTR(L"ROOT\\CIMV2"),
      NULL, NULL, 0, NULL, 0, 0, &m_pWbemSvc);
    VERIFY(SUCCEEDED(hr));

    // 设置 WMI 连接安全性
    hr = CoSetProxyBlanket(m_pWbemSvc, RPC_C_AUTHN_WINNT, RPC_C_AUTHZ_NONE,
      NULL, RPC_C_AUTHN_LEVEL_CALL, RPC_C_IMP_LEVEL_IMPERSONATE,
      NULL, EOAC_NONE);
    VERIFY(SUCCEEDED(hr));
  }
  return hr;
}

HRESULT CWmiInfo::ReleaseWmi()
{
  HRESULT hr;
  if (NULL != m_pWbemSvc) hr = m_pWbemSvc->Release();
  if (NULL != m_pWbemLoc) hr = m_pWbemLoc->Release();
  if (NULL != m_pEnumClsObj) hr = m_pEnumClsObj->Release();
  ::CoUninitialize();
  return hr;
}

BOOL CWmiInfo::GetSingleItemInfo(CString ClassName, CString ClassMember, CString& chRetValue)
{
  USES_CONVERSION;
  CComBSTR query("SELECT * FROM ");
  VARIANT vtProp;
  ULONG uReturn;
  HRESULT hr;
  BOOL bRet = FALSE;

  if (NULL != m_pWbemSvc)
  {
    query += CComBSTR(ClassName);
    hr = m_pWbemSvc->ExecQuery(CComBSTR("WQL"), query,
      WBEM_FLAG_FORWARD_ONLY | WBEM_FLAG_RETURN_IMMEDIATELY, 0, &m_pEnumClsObj);
    
    if (SUCCEEDED(hr))
    {
      VariantInit(&vtProp);
      uReturn = 0;
      hr = m_pEnumClsObj->Next(WBEM_INFINITE, 1, &m_pWbemClsObj, &uReturn);
      
      if (SUCCEEDED(hr) && uReturn > 0)
      {
        hr = m_pWbemClsObj->Get(CComBSTR(ClassMember), 0, &vtProp, 0, 0);
        if (SUCCEEDED(hr))
        {
          VariantToString(&vtProp, chRetValue);
          VariantClear(&vtProp);
          bRet = TRUE;
        }
      }
    }
  }

  if (NULL != m_pEnumClsObj) { m_pEnumClsObj->Release(); m_pEnumClsObj = NULL; }
  if (NULL != m_pWbemClsObj) { m_pWbemClsObj->Release(); m_pWbemClsObj = NULL; }
  return bRet;
}

void CWmiInfo::VariantToString(const LPVARIANT pVar, CString& chRetValue) const
{
  USES_CONVERSION;
  switch (pVar->vt)
  {
  case VT_BSTR:
    chRetValue = W2T(pVar->bstrVal);
    break;
  case VT_BOOL:
    chRetValue = (VARIANT_TRUE == pVar->boolVal) ? "是" : "否";
    break;
  case VT_I4:
    chRetValue.Format(_T("%d"), pVar->lVal);
    break;
  case VT_UI1:
    chRetValue.Format(_T("%d"), pVar->bVal);
    break;
  case VT_UI4:
    chRetValue.Format(_T("%d"), pVar->ulVal);
    break;
  default:
    break;
  }
}
```

#### MFC 使用示例

```cpp
CWmiInfo info;
info.InitWmi();

CString uuid, processorId, baseboardSerialNumber;
CString baseboardProduct, baseboardManufacturer, biosSerialNumber;

info.GetSingleItemInfo(L"Win32_ComputerSystemProduct", L"UUID", uuid);
info.GetSingleItemInfo(L"Win32_Processor", L"ProcessorId", processorId);
info.GetSingleItemInfo(L"Win32_BaseBoard", L"SerialNumber", baseboardSerialNumber);
info.GetSingleItemInfo(L"Win32_BaseBoard", L"Product", baseboardProduct);
info.GetSingleItemInfo(L"Win32_BaseBoard", L"Manufacturer", baseboardManufacturer);
info.GetSingleItemInfo(L"Win32_BIOS", L"SerialNumber", biosSerialNumber);

// 拼接后计算哈希作为机器码
CString total = uuid + L"\r\n" + processorId + L"\r\n" + baseboardSerialNumber 
  + L"\r\n" + baseboardProduct + L"\r\n" + baseboardManufacturer + L"\r\n" + biosSerialNumber;

info.ReleaseWmi();
```

### Win32 版本

#### wmi_info.h

```cpp
#pragma once
#include <list>
#include <string>

namespace base {
namespace wmi {
  bool GetBIOSUUID(std::string& result);
  bool GetProcessorId(std::string& result);
  bool GetBaseBoardSerialNumber(std::string& result);
  bool GetBaseBoardProduct(std::string& result);
  bool GetBaseBoardManufacturer(std::string& result);
  bool GetBIOSSerialNumber(std::string& result);
  bool GetDiskSerialNumber(std::string& result);
}
}
```

#### wmi_info.cpp

```cpp
#include "base/wmi_info.h"
#include <Windows.h>
#include <vector>
#include <algorithm>

namespace base {
namespace wmi {

std::string ExeCmd(std::wstring pszCmd)
{
  SECURITY_ATTRIBUTES sa = { sizeof(SECURITY_ATTRIBUTES), NULL, TRUE };
  HANDLE hRead, hWrite;
  if (!CreatePipe(&hRead, &hWrite, &sa, 0)) return "";

  STARTUPINFO si = { sizeof(STARTUPINFO) };
  GetStartupInfo(&si);
  si.dwFlags = STARTF_USESHOWWINDOW | STARTF_USESTDHANDLES;
  si.wShowWindow = SW_HIDE;
  si.hStdError = hWrite;
  si.hStdOutput = hWrite;

  PROCESS_INFORMATION pi;
  if (!CreateProcessW(NULL, const_cast<LPWSTR>(pszCmd.c_str()), 
      NULL, NULL, TRUE, NULL, NULL, NULL, &si, &pi))
    return "";

  CloseHandle(hWrite);

  std::string strRet;
  char buff[1024] = { 0 };
  DWORD dwRead = 0;
  while (ReadFile(hRead, buff, 1024, &dwRead, NULL)) {
    strRet.append(buff, dwRead);
  }

  CloseHandle(pi.hProcess);
  CloseHandle(pi.hThread);
  CloseHandle(hRead);
  return strRet;
}

// ... 辅助函数省略，完整代码见原文件

bool GetBIOSUUID(std::string& result) {
  return GetFirstStringValue(L"wmic csproduct get UUID", result);
}

bool GetProcessorId(std::string& result) {
  return GetFirstStringValue(L"wmic cpu get processorid", result);
}

bool GetBaseBoardSerialNumber(std::string& result) {
  return GetFirstStringValue(L"wmic baseboard get serialnumber", result);
}

bool GetBaseBoardProduct(std::string& result) {
  return GetFirstStringValue(L"wmic baseboard get Product", result);
}

bool GetBaseBoardManufacturer(std::string& result) {
  return GetFirstStringValue(L"wmic baseboard get Manufacturer", result);
}

bool GetBIOSSerialNumber(std::string& result) {
  return GetFirstStringValue(L"wmic bios get serialnumber", result);
}

bool GetDiskSerialNumber(std::string& result) {
  return GetFirstStringValue(L"wmic diskdrive get serialnumber", result);
}

}
}
```

#### Win32 使用示例

```cpp
std::string uuid, processorId, baseBoardSerialNumber;
std::string baseBoardProduct, baseBoardManufacturer, biosSerialNumber;

base::wmi::GetBIOSUUID(uuid);
base::wmi::GetProcessorId(processorId);
base::wmi::GetBaseBoardSerialNumber(baseBoardSerialNumber);
base::wmi::GetBaseBoardProduct(baseBoardProduct);
base::wmi::GetBaseBoardManufacturer(baseBoardManufacturer);
base::wmi::GetBIOSSerialNumber(biosSerialNumber);

// 拼接后计算 MD5 作为机器码
std::string total = uuid + "@" + processorId + "@" + baseBoardSerialNumber 
  + "@" + baseBoardProduct + "@" + baseBoardManufacturer + "@" + biosSerialNumber;
std::string hash = md5(total.c_str());
```
