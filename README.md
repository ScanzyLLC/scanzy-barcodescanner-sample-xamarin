# scanzy-barcodescanner-sample-xamarin
scanzy barcode scanner sample for xamarin (native iOS, Android)

# Xamarin Native

# iOS

Install nuget package ScanzyBarcodeScannerSDK from visual studio nuget manager.


To use this plugin in Javascript:

Firstly, set the license, it's better to do it in your app's startup, although it's fine to call this function every single time to scan the barcode.

```csharp

ScanzyBSLicense.SetLicense("your-valid-licensekey");

```

To get the barcode result from SDK, you should implement the delegate and override the GetBarcode method to follow your actual logic, such as display the alert dialog. For example,

```csharp

    public class BarcodeDelegate : ScanzyBarcodeScannedProtocolDelegate
    {
        private UIViewController _uIViewController;

        public BarcodeDelegate(UIViewController uIViewController)
        {
            this._uIViewController = uIViewController;
        }

        public override void GetBarcode(string barcode)
        {
            var okAlertController = UIAlertController.Create("Barcode", barcode, UIAlertControllerStyle.Alert);
            okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
            this._uIViewController.PresentViewController(okAlertController, true, null);
        }
    }

```

Then, insert below code snippet into the place to scan barcode, such as button click event:

```csharp

  //support Code128, Ean13
  
   ScanzyBSBarcodeFormat format = ScanzyBSBarcodeFormat.Code128 | ScanzyBSBarcodeFormat.Ean13;

   //enableVibration: true, vibrate your phone when barcode detected
   //enableBeep: true, play the beep sound when barcode detected
   //enableAutoZoom: the library will zoom in/out automatcially to scan the barcode
   //enableScanRectOnly: only scan the view finder area
   ScanzyBSBarcodeOptions options = new ScanzyBSBarcodeOptions(format,true,true,true,false);
   ScanzyBSBarcodePicker picker = new ScanzyBSBarcodePicker(options);
   mydelegate = new BarcodeDelegate(this);
   picker.Delegate = mydelegate;
            
   this.PresentViewController(picker, true, null);
  
```
# Android

Install nuget package ScanzyBarcodeScannerSDK from visual studio nuget manager.


To use this plugin in Javascript:

Firstly, set the license, it's better to do it in your app's startup, although it's fine to call this function every single time to scan the barcode.

```csharp

 ScanzyBSLicense.SetLicense(this.ApplicationContext,"Your-license-key");

```

Then, insert below code snippet into the place to scan barcode, such as button click event:

```csharp

   //enableVibration: true, vibrate your phone when barcode detected
   //enableBeep: true, play the beep sound when barcode detected
   //enableAutoZoom: the library will zoom in/out automatcially to scan the barcode
   //enableScanRectOnly: only scan the view finder area
   //support Code128, Ean13
   ScanzyBSBarcodeOptions barcodeOptions = new ScanzyBSBarcodeOptions(
                true,true,true,false,EnumSet.Of(
                    ScanzyBSBarcodeFormat.Ean13,
                    ScanzyBSBarcodeFormat.Code128));

            ScanzyBSBarcodeManager manager = new ScanzyBSBarcodeManager(this.ApplicationContext,barcodeOptions);
            var intent = manager.GetBarcodeScannerIntent(this);

            StartActivityForResult(intent, ScanzyBSBarcodeManager.RcBarcodeCapture);
             
```

To get the barcode result from SDK, you should override the OnActivityResult. For example,

```csharp

     protected override void OnActivityResult(int requestCode, [GeneratedEnum] Result resultCode, Android.Content.Intent data)
        {
            if(requestCode == ScanzyBSBarcodeManager.RcBarcodeCapture)
            {
                if(resultCode == BarcodeScanStatus.Success)
                {
                    if(data != null)
                    {
                        string barcode = data.GetStringExtra("barcode");
                        if(barcode.Equals("permission_missing"))
                        {
                            //You don't have valid license key
                        }
                        else
                        {
                            Android.App.AlertDialog.Builder alertDialog = new Android.App.AlertDialog.Builder(this);
                            alertDialog.SetMessage(barcode).SetTitle("SCAN RESULT");
                            Android.App.AlertDialog dialog = alertDialog.Create();
                            dialog.Show();
                        }
                    }
                }
            }

            base.OnActivityResult(requestCode, resultCode, data);
        }

```

That is all you need to use the plugin. :joy:

