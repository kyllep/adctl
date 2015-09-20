# adctl
Qt for Google Analytics, Google AdMob, Google Play services (auth and achievements) and StartAd.mobi framework

Completed library features:
- Qt and Google AdMob (based on yevgeniy-logachev/QtAdMob https://github.com/yevgeniy-logachev/QtAdMob). Work on Android/iOS
- Qt and StartAd.mobi (based on https://github.com/kafeg/SDK-Android). Work only Android
- Qt and Google Analytics (based on StartAD/SDK-Android https://github.com/HSAnet/qt-google-analytics). Work on Android/iOS/Desktop

All functions tested on 3 Android devices, but it not tested on iOS! Library give you C++ and QML interfaces for using.

In my feature plan:
- Add more cross-promoutin Ads;
- Add Google Play authorization from VoltAir (https://github.com/google/VoltAir);
- Add Google Play Achievements from VoltAir (https://github.com/google/VoltAir);
- Add support StartAd.mobi on iOS.

Known issues:
- need add StartAd.mobi iOS integration
- need to refactor copy data functions in AdCtl.pri for copy data into build folder (now its auto copy to project folder)
- need to refactor copy data funiction in AdCtl.pri for copy data only once time (now its copy on every build)

Howto integrate library to project:

1. Add submodule to your project:
```
    - mkdir $$PROJECT_ROOT/mobile
    - cd $$PROJECT_ROOT/mobile
    - git submodule add https://github.com/kafeg/adctl.git
    - git submodule update --init --recursive
```

2. Move your 'android' folder to $$PROJECT_ROOT/mobile/android (for example, it's not required)

3. Include .pri in your project:

```
    #AdCtl: Google Analytics, AdMob, StartAD.mobi
    ANDROID_PACKAGE_SOURCE_DIR = $$PWD/mobile/android
    include(mobile/adctl/AdCtl.pri)
    android {
      OTHER_FILES += \
        $$PWD/mobile/android/AndroidManifest.xml
    }
```
4. That's all!

Howto using library from C++

1. in main.cpp (for example) add:
```
#include <mobile/adctl/adctl.h>

...

//AdCtl
QApplication::setApplicationName("Darkstories");
QApplication::setApplicationVersion("1.1");
AdCtl *adCtl = new AdCtl();
adCtl->setAdMobBannerEnabled(true);
...see example for setting variables in QML section and in adctl.h...
adCtl->init();
```
Howto using library from Qml

1. In main.cpp add:
```
    #include <mobile/adctl/adctl.h>
    
    ...
    
    //AdCtl
    QApplication::setApplicationName("Darkstories");
    QApplication::setApplicationVersion("1.1");
    qmlRegisterType<AdCtl>("ru.forsk.adctl", 1, 0, "AdCtl");
```
2. In main.qml add:
```
    AdCtl {
        id: adCtl
    
        //manage enabled components
        adMobBannerEnabled: true
        adMobIinterstitialEnabled: true
        startAdBannerEnabled: true
        gAnalyticsEnabled: true
    
        //set ids
        adMobId: "YOUR_ADMOB_UNIT_ID"
        startAdId: "YOUR_STARTADMOBI_ID"
        gAnalyticsId: "YOUR_GANALYTICS_TRACKING_ID"
    
        //Start positions for banners.
        adMobBannerPosition: Qt.point(0,-500)
        startAdBannerPosition: Qt.point(0,-500)
    
        //when StartAd.mobi baners is showed we can to reposition it
        onStartAdBannerShowed: {
            console.log("onStartAdBannerShowed");
            startAdBannerPosition = Qt.point(0,
                                     (appWindow.height - adCtl.startAdBannerHeight * 1.3))
        }
    
        //when AdMob baners is showed we can to reposition it
        onAdMobBannerShowed: {
            console.log("onAdMobBannerShowed");
            adMobBannerPosition = Qt.point((appWindow.width - adCtl.adMobBannerWidth) * 0.5,
                                     (appWindow.height - adCtl.adMobBannerHeight * 1.5 - 200))
            adCtl.showAdMobInterstitial();
        }
    
        //When all variables are setted, we can to initialize our code
        Component.onCompleted: { adCtl.init(); adCtl.showAdMobInterstitial(); }
    }
```
3. For example, interact with AdCtl:
```
    Rectangle {
        id: root
    
        anchors.fill: parent
        anchors.bottomMargin: adCtl.startAdBannerHeight
    
    }
```

Developer:
kafeg aka Vitaliy Petrov
Skype: kafik-fafik
EMail: v31337[at]gmail.com

My projects:
- http://forsk.ru - адекватная автоматизация бизнес процессов.
- http://skid.kz - автоматический агрегатор скидок Республики Казахстан.
- http://kellot.ru - онлайн табель учёта рабочего времени по формам Т-12 и Т-13.
- https://github.com/kafeg

Details of using this library in Russian: TODO
