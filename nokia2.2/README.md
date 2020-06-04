# Nokia 2.2
https://en.wikipedia.org/wiki/Nokia_2.2

A decent phone with 
  - Android one
  -  removable 3000 mAh battery
  -  microUSB port.

# Forums
https://community.phones.nokia.com/categories/nokia-2-2

# Remove junk (a.k.a. Play Services)

  - Import a HTML file and watch it magically convert to Markdown
  - Drag and drop images (requires your Dropbox account be linked)

### Enable 3 button navbar
https://community.phones.nokia.com/discussion/57250/guide-to-enable-3-button-navigation-in-android-10

  - set quickstep as default launcher
  - enable gesture navigation instead of 2 button
  - install another launcher like lawnchair and enable it as default.
  - The next command may be not needed.
```sh
pm disable-user --user 0 com.android.launcher3
```




### Remove crap

Taken from  XDA link https://forum.xda-developers.com/nokia-7-plus/how-to/stock-disabling-crap-device-ota-t3906929

Run this inside ```adb shell``` after enabling developer in settings.

```sh
pm disable-user --user 0  com.google.android.apps.turbo
pm uninstall --user 0 com.google.android.apps.turbo
pm disable-user --user 0 com.evenwell.DbgCfgTool.overlay.base.s600ww
pm disable-user --user 0 com.android.cts.priv.ctsshim
pm disable-user --user 0 com.qualcomm.qti.qms.service.telemetry
pm disable-user --user 0 com.google.android.youtube
pm disable-user --user 0 com.evenwell.SetupWizard.overlay.base.s600ww
pm disable-user --user 0 com.google.android.ext.services
pm disable-user --user 0 com.google.android.googlequicksearchbox
pm disable-user --user 0 com.hmdglobal.datago
pm disable-user --user 0 com.evenwell.phone.overlay.base.s600ww
pm disable-user --user 0 com.google.android.onetimeinitializer
pm disable-user --user 0 com.evenwell.permissiondetection.overlay.base.s600ww
pm disable-user --user 0 com.google.android.ext.shared
pm disable-user --user 0 com.android.stk.base.s600ww
pm disable-user --user 0 com.android.wallpapercropper
pm disable-user --user 0 com.android.carrierconfig.auto_generated_rro__
pm disable-user --user 0 com.android.bluetooth.auto_generated_rro__
pm disable-user --user 0 com.evenwell.pushagent.overlay.base.s600ww
pm disable-user --user 0 android.auto_generated_rro__
pm disable-user --user 0 com.qualcomm.qti.qms.service.connectionsecurity
pm disable-user --user 0 com.android.systemui.overlay.base.s600ww
pm disable-user --user 0 com.google.android.apps.messaging
pm disable-user --user 0 android.framework.base.s600ww
pm disable-user --user 0 com.evenwell.bboxsbox
pm disable-user --user 0 com.evenwell.partnerbrowsercustomizations.overlay.base.s600ww
pm disable-user --user 0 com.evenwell.permissiondetection
pm disable-user --user 0 com.android.server.telecom.auto_generated_rro__
pm disable-user --user 0 com.evenwell.weatherservice.overlay.base.s600ww
pm disable-user --user 0 com.qualcomm.qti.optinoverlay
pm disable-user --user 0 org.codeaurora.bluetooth.auto_generated_rro__
pm disable-user --user 0 com.evenwell.factorywizard
pm disable-user --user 0 com.evenwell.retaildemoapp.overlay.base.s600ww
pm disable-user --user 0 com.google.ar.lens
pm disable-user --user 0 com.evenwell.PowerMonitor.overlay.base.s600ww
pm disable-user --user 0 com.android.vending
pm disable-user --user 0 com.android.settings.overlay.cmcc
pm disable-user --user 0 com.evenwell.foxlauncher.partner
pm disable-user --user 0 com.evenwell.round
pm disable-user --user 0 com.android.retaildemo.overlay.base.s600ww
pm disable-user --user 0 com.google.android.marvin.talkback
pm disable-user --user 0 com.google.android.apps.work.oobconfig
pm disable-user --user 0 com.android.cellbroadcastreceiver.overlay.base.s600ww
pm disable-user --user 0 com.android.mms.overlay.cmcc
pm disable-user --user 0 android.ui.overlay.ct
pm disable-user --user 0 com.android.stk
pm disable-user --user 0 com.hmdglobal.camera2
pm disable-user --user 0 com.fih.StatsdLogger
pm disable-user --user 0 com.evenwell.telecom.data.overlay.base.s600ww
pm disable-user --user 0 com.google.android.as
pm disable-user --user 0 com.google.android.gm
pm disable-user --user 0 com.evenwell.fqc
pm disable-user --user 0 com.evenwell.nps
pm disable-user --user 0 com.evenwell.bokeheditor.overlay.base.s600ww
pm disable-user --user 0 com.evenwell.stbmonitor.data.overlay.base.s600ww
pm disable-user --user 0 com.fih.infodisplay
pm disable-user --user 0 com.google.android.setupwizard
pm disable-user --user 0 com.evenwell.autoregistration
pm disable-user --user 0 com.android.printspooler
pm disable-user --user 0 com.evenwell.legalterm
pm disable-user --user 0 com.evenwell.settings.data.overlay.base.s600ww
pm disable-user --user 0 com.android.dreams.basic
pm disable-user --user 0 com.google.android.apps.wellbeing
pm disable-user --user 0 com.android.retaildemo
pm disable-user --user 0 com.evenwell.OTAUpdate.overlay.base.s600ww
pm disable-user --user 0 com.android.bips
pm disable-user --user 0 com.evenwell.stbmonitor
pm disable-user --user 0 com.google.android.apps.nbu.files
pm disable-user --user 0 com.android.settings.btl.s600ww.overlay
pm disable-user --user 0 com.evenwell.PowerMonitor
pm disable-user --user 0 com.evenwell.DbgCfgTool
pm disable-user --user 0 com.android.partnerbrowsercustomizations.btl.s600ww.overlay
pm disable-user --user 0 com.android.systemui.overlay.cmcc
pm disable-user --user 0 com.evenwell.batteryprotect.overlay.base.s600ww
pm disable-user --user 0 com.google.android.apps.docs
pm disable-user --user 0 com.google.android.apps.maps
pm disable-user --user 0 com.evenwell.AprUploadService
pm disable-user --user 0 com.evenwell.UsageStatsLogReceiver.data.overlay.base.s600ww
pm disable-user --user 0 com.evenwell.batteryprotect
pm disable-user --user 0 android.telephony.overlay.cmcc
pm disable-user --user 0 android.btl.s600ww.overlay
pm disable-user --user 0 com.google.android.contacts
pm disable-user --user 0 com.google.android.syncadapters.contacts
pm disable-user --user 0 com.evenwell.factorywizard.overlay.base.s600ww
pm disable-user --user 0 com.evenwell.dataagent.overlay.base.s600ww
pm disable-user --user 0 com.android.facelock
pm disable-user --user 0 com.android.chrome
pm disable-user --user 0 com.evenwell.nps.overlay.base.s600ww
pm disable-user --user 0 com.google.android.gms
pm disable-user --user 0 com.google.android.gsf
pm disable-user --user 0 com.google.android.ims
pm disable-user --user 0 com.google.android.tag
pm disable-user --user 0 com.google.android.tts
pm disable-user --user 0 com.hmdglobal.datago.overlay.base.s600ww
pm disable-user --user 0 com.android.calllogbackup
pm disable-user --user 0 com.google.android.partnersetup
pm disable-user --user 0 com.evenwell.bboxsbox.app
pm disable-user --user 0 com.google.android.videos
pm disable-user --user 0 com.evenwell.CPClient
pm disable-user --user 0 com.android.partnerbrowsercustomizations
pm disable-user --user 0 com.google.android.feedback
pm disable-user --user 0 com.google.android.printservice.recommendation
pm disable-user --user 0 com.google.android.apps.photos
pm disable-user --user 0 com.google.android.calendar
pm disable-user --user 0 com.android.phone.auto_generated_rro__
pm disable-user --user 0 com.evenwell.powersaving.g3.overlay.base.s600ww
pm disable-user --user 0 com.android.systemui.auto_generated_rro__
pm disable-user --user 0 com.android.dreams.phototable
pm disable-user --user 0 com.android.networksettings.overlay.ct
pm disable-user --user 0 com.android.wallpaper.livepicker
pm disable-user --user 0 com.evenwell.custmanager.data.overlay.base.s600ww
pm disable-user --user 0 com.evenwell.CPClient.overlay.base.s600ww
pm disable-user --user 0 com.evenwell.powersaving.g3
pm disable-user --user 0 com.evenwell.SetupWizard
pm disable-user --user 0 com.evenwell.bokeheditor
pm disable-user --user 0 com.google.android.gms.policy_sidecar_aps
pm disable-user --user 0 com.google.android.backuptransport
pm disable-user --user 0 com.evenwell.DeviceMonitorControl
pm disable-user --user 0 com.evenwell.AprUploadService.data.overlay.base.s600ww
pm disable-user --user 0 com.evenwell.custmanager
pm disable-user --user 0 com.evenwell.providers.partnerbookmarks.overlay.base.s600ww
pm disable-user --user 0 com.evenwell.pushagent
pm disable-user --user 0 com.evenwell.managedprovisioning.overlay.base.s600ww
pm disable-user --user 0 com.hmdglobal.support
pm disable-user --user 0 com.android.launcher3.overlay.base.s600ww
pm disable-user --user 0 com.evenwell.providers.weather.overlay.base.s600ww
pm disable-user --user 0 com.android.cts.ctsshim
pm disable-user --user 0 com.android.cellbroadcastreceiver.auto_generated_rro__
pm disable-user --user 0 com.hmdglobal.appstore.overlay.base.s600ww
pm disable-user --user 0 com.android.systemui.overlay.ct
pm disable-user --user 0 com.hmdglobal.camera2.overlay.base.s600ww
pm disable-user --user 0 com.google.android.apps.wallpaper
pm disable-user --user 0 com.android.providers.settings.overlay.base.s600ww
pm disable-user --user 0 com.android.wallpaperbackup
pm disable-user --user 0 com.evenwell.DeviceMonitorControl.data.overlay.base.s600ww
pm disable-user --user 0 com.evenwell.dataagent
pm disable-user --user 0 com.android.providers.userdictionary
pm disable-user --user 0 com.evenwell.customerfeedback.overlay.base.s600ww
pm disable-user --user 0 com.android.documentsui.overlay.base.s600ww
pm disable-user --user 0 com.evenwell.legalterm.overlay.base.s600ww
pm disable-user --user 0 com.evenwell.retaildemoapp
pm disable-user --user 0 com.android.providers.calendar.overlay.base.s600ww
pm disable-user --user 0 com.nbc.intellirecog.overlay.base.s600ww
pm disable-user --user 0 com.evenwell.mappartner
pm disable-user --user 0 com.android.providers.settings.btl.s600ww.overlay
pm disable-user --user 0 com.google.android.apps.restore
```

# Warning
Do not run the next command without verifying. Some packages with 'com.google.android' is needed for normal phone operation. Otherwise phone crashes boots to recovery. Then, just factory reset.

```sh
for crap in $(pm list packages | grep "evenwell\|google" | cut -d ":" -f2)
do
pm uninstall --user 0 $crap
done
```

