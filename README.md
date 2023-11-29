# Create Mobile Native Popup Using Unity
## Platform
- iOS
- Android
## Feature
- Ok, Yes/No, Neutral popup (Single, two, three action)

<img src="https://github.com/j1mmyto9/unity-mobile-dialog-popup-native/blob/main/Img/AndroidBox.png"><img src="https://github.com/j1mmyto9/unity-mobile-dialog-popup-native/blob/main/Img/iOSBox.png">

- DatePicker, TimePicker

<img src="https://github.com/j1mmyto9/unity-mobile-dialog-popup-native/blob/main/Img/AndroidDate.png"><img src="https://github.com/j1mmyto9/unity-mobile-dialog-popup-native/blob/main/Img/iOSDate.png">

## Unity에서 쉽게 액세스
iOS 및 Android의 네이티브 기능을 Unity에서 쉽게 액세스할 수 있도록 하는 Native Popups에 오신 것을 환영합니다! 이 플러그인은 Unity에서 iOS 및 Android의 네이티브 기능을 사용하여 팝업을 표시하는 데 사용됩니다.
```csharp
    public void OnDialogInfo()
    {
        NativeDialog.OpenDialog("Info popup", "Welcome To Native Popup", "Ok", 
            ()=> {
                Debug.Log("OK Button pressed");
            });
    }

   

    public void OnDatePicker()
    {
        NativeDialog.OpenDatePicker(1992,5,10,
            (DateTime _date) =>
            {
                Debug.Log(_date.ToString());
            },
            (DateTime _date) =>
            {
                Debug.Log(_date.ToString());
            });        
    }
```
## Open source
모든 java 및 objective c 소스를 제공했습니다. 작동 방식, 최적화 또는 기능 추가 방법을 알 수 있습니다.

### Android studio
- Unity call to static function
```csharp
    AndroidJavaClass javaUnityClass = new AndroidJavaClass("com.pingak9.nativepopup.Bridge");
    javaUnityClass.CallStatic("ShowDialogInfo", title, message, ok);
```
- Java show popup
```objectivec
    void ShowDialogInfo(String title, String message, String ok) {

        AlertDialog alertDialog = new AlertDialog.Builder(this).create(); //Read Update
        alertDialog.setTitle(title);
        alertDialog.setMessage(message);

        alertDialog.setButton(ok, new DialogInterface.OnClickListener() {
            public void onClick(DialogInterface dialog, int which) {
                UnityPlayer.UnitySendMessage("MobileDialogInfo", "OnOkCallBack", "0");
            }
        });

        alertDialog.show();
    }
```
- 라이브러리 JAR 빌드 및 JAR 파일을 Plugins/Android 폴더에 복사
### Objective C
- Unity call to C
```csharp
    [DllImport("__Internal")]
    private static extern void _TAG_ShowDialogInfo(string title, string message, string ok);
```
- Objective C show popup
```objectivec
+(void)ShowDialogInfo: (NSString *) title message: (NSString*) msg okTitle:(NSString*) b1 {
    
    UIAlertController *alertController = [UIAlertController alertControllerWithTitle:title message:msg preferredStyle:UIAlertControllerStyleAlert];
    
    UIAlertAction *okAction = [UIAlertAction actionWithTitle:b1 style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
        [IOSNativePopUpsManager unregisterAllertView];
        UnitySendMessage("MobileDialogInfo", "OnOkCallBack",  [DataConvertor NSIntToChar:0]);
    }];
    [alertController addAction:okAction];
    
    
    [[[[UIApplication sharedApplication] keyWindow] rootViewController] presentViewController:alertController animated:YES completion:nil];
    _currentAllert = alertController;
}
```
- Objective C 파일을 Plugins/iOS 폴더에 복사


좋은 코딩 되세요!
