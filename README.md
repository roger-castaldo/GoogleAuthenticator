# GoogleAuthenticator
Form of Simple, easy to use server-side two-factor authentication library for .NET that works with Google Authenticator.  This fork allows for usage of either QRCoder or SkiaSharp.QRCode to generate the QR Code.  In order to generate a QRCode you must include the reference 
for either package.  

Currently Supported Versions:
QRCoder: 1.4.3
SkiaSharp.QRCode: 0.6.0

[`Install-Package GoogleAuthenticator`](https://www.nuget.org/packages/GoogleAuthenticator.MutlipleQRCoder)

## 2.x Usage

*Additional examples at [Google.Authenticator.WinTest](https://github.com/roger-castaldo/GoogleAuthenticator/tree/master/Google.Authenticator.WinTest) and [Google.Authenticator.WebSample](https://github.com/roger-castaldo/GoogleAuthenticator/tree/master/Google.Authenticator.WebSample)*

```csharp
using Google.Authenticator;

string key = Guid.NewGuid().ToString().Replace("-", "").Substring(0, 10);

TwoFactorAuthenticator tfa = new TwoFactorAuthenticator();
SetupCode setupInfo = tfa.GenerateSetupCode("Test Two Factor", "user@example.com", key, false, 3);

string qrCodeImageUrl = setupInfo.QrCodeSetupImageUrl;
string manualEntrySetupCode = setupInfo.ManualEntryKey;

imgQrCode.ImageUrl = qrCodeImageUrl;
lblManualSetupCode.Text = manualEntrySetupCode;

// verify
TwoFactorAuthenticator tfa = new TwoFactorAuthenticator();
bool result = tfa.ValidateTwoFactorPIN(key, txtCode.Text)
```

## Updates

### 2.5.0
Now runs on .Net 6.0.  
Technically the QR Coder library we rely on still does not fully support .Net 6.0 so it is possible there will be other niggling issues, but for now all tests pass for .Net 6.0 on both Windows and Linux.

## Common Pitfalls

* Old documentation indicated specifying width and height for the QR code, but changes in QR generation now uses pixels per module (QR "pixel") so using a value too high will result in a huge image that can overrun memory allocations
* Don't use the secret key and `ManualEntryKey` interchangeably. `ManualEntryKey` is used to enter into the authenticator app when scanning a QR code is impossible and is derived from the secret key ([discussion example](https://github.com/BrandonPotter/GoogleAuthenticator/issues/54))

# Notes
On linux, if you use QRCoder instead of SkiaSharp.QRCode, you need to ensure `libgdiplus` is installed if you want to generate QR Codes. See [https://github.com/codebude/QRCoder/issues/227](https://github.com/codebude/QRCoder/issues/227).
