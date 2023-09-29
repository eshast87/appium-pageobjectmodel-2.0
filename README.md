# appium-pageobjectmodel

Technologies/Tools used in building the framework
=================================================
- Eclipse - IDE
- Appium - Mobile Automation library
- Maven - Build automation tool
- Java - Programming language
- TestNG - Test Management library
- Log4J - Logging framework
- Extent Reports - Reporting framework
- JSON/Excel - Test Data
- XML - Static text
- GitHub - Version control
- Jenkins - CI/CD

Framework implements below best practices
=========================================
- Code reusability
- Code readability
- Scalable automation (demonstrated using multiple test classes)
- Uses explicit waits
- Abstraction layer for UI commands like click, sendkeys, etc.
- Parameterization using TestNG XML and config.properties
- Alternate Design approach [Without using inheritance]
- Exception handling [using Try/Catch and TestNG Listener]
- Abstraction layer for test data
- Abstraction layer for static text
- Supports iOS and Android
- Demonstrates how to define UI elements that are common across pages (e.g. menu bar, side bar, etc.)
- How to recover from test failure/ how to write fail safe test cases
- Scrolling for both Android and iOS (using touchaction, uiScrollable, mobile:scroll)
- Demonstrates how to effectively capture Screenshots/Videos
- Supports parallel execution on multiple real Android and iOS devices
- Integrated with Log4J2 Logging framework (supports basic as well as parallel logging)
- Integrated with Extent Reporting framework (supports parallel, screenshots, logging test steps)

#AppiumTutorials #PageObjectModel #TestNG

# Video for Demo
https://drive.google.com/file/d/1DpeFeha7ujZVvXRHkeamAeQI9GRTmMOb/view?usp=sharing
#MobileTesting
![Mobile Application Testing Strategy ](https://github.com/eshast87/appium-pageobjectmodel-2.0/assets/20563849/f6275483-14c8-4626-b0ed-00f71a8934c6)

# iOS setup: 
Important setup notes
================= 
-> Be an admin on your Windows/Mac
-> Use latest Windows/MacOS operating system
-> Office machine? make sure anti-virus and company policies are not blocking installation of Appium and associated softwares
-> If practicing using an Android emulator, use a powerful processor and sufficient RAM
-> Avoid using phone from Chinese manufacturers that may restrict Appium due to their security limitations


Install Xcode
====================
Configure Apple ID in Account preferences
Install from App Store


Install Xcode command line tools
======================================
Command: xcode-select --install


Install xcpretty [to make Xcode output reasonable]
========================================================
Command to install xcpretty: gem install xcpretty


Install Carthage [dependency manager, required for WebDriverAgent]
==================================================================
Note: This may be optional. In case of any installation issue, just ignore and proceed further.
Command to install Carthage: brew install Carthage


Install Appium-doctor and check Appium setup
===================================================
Command to install Appium doctor: npm install -g appium-doctor
Command to get help: appium-doctor --h
Command to check setup for iOS: appium-doctor --ios


Install XCUITest driver (using Appium CLI)
===========================================
Command to get help: appium driver --help (or -h)
Command to list drivers: appium driver list
Command to install driver: appium driver install XCUITest


Simulator setup: Build UIKitCatalog app for simulator
=====================================================
Download link: https://github.com/appium/ios-uicatalog
Command to get simulator name: xcodebuild -showsdks
Command to build app for the simulator: xcodebuild -sdk <simulator_name>
Command to build UIKitCatalog app for simulator: npm install


Simulator setup: Start session with UIKitCatalog app using Appium CLI
=======================================================================
Command to get UDID: xcrun simctl list
Command to get UDID: xcrun xctrace list devices
Xcode option to get UDID: Xcode -> Window -> Devices and Simulators -> Simulators -> Select the simulator in the left pane. In the right pane, "Identifier" will be the UDID. 


=================================================================
                       Real device setup
=================================================================

Getting UDID
=============
Command to install ios-deploy: npm install -g ios-deploy
Command to get UDID: ios-deploy -c
OR
Command to get UDID: xcrun simctl list [May show real devices as well]
Command to get UDID: xcrun xctrace list devices [May show real devices as well]
Xcode option to get UDID: Xcode -> Window -> Devices and Simulators -> Devices -> Select the device in the left pane. In the right pane, "Identifier" will be the UDID. 


Option 1. Code signing WebDriverAgent: Basic (automatic/manual) configuration.
=============================================================================

Step 1. Enrol for Developer program
-> Create Apple account: https://developer.apple.com
-> Enable two factor authentication: https://appleid.apple.com/account/manage
-> Click Join the Apple Developer program
-> Click Enrol
-> Click Start Your Enrolment

Step 2. Register device UDID on the developer portal (this can be done from Xcode as well)

Step 3. Add your Apple ID (paid developer account) to Xcode and download the certificate (if required, create new certificate on the developer portal)

Step 4. Generate UIKitCatalog IPA:
-> Download UIKitCatalog app from https://github.com/appium/ios-uicatalog
-> Launch project and code sign using Xcode managed provisioning profile
-> Confirm wild card provisioning profile is created in the developer account
-> (select generic iOS device) Archive app to generate IPA

Step 5. Create session with app using Appium CLI server
-> Open inspector session and set below desired capabilities
"xcodeOrgId": "your team id"
"xcodeSigningId": "iPhone Developer"


Option 2. Code signing WebDriverAgent: Full manual configuration path
====================================================================
Step 1. Add your Apple ID to Xcode and download the certificate

Step 2. Find WebDriverAgent project
In terminal, execute command:
echo "$(dirname "$(find "$HOME/.appium" -name WebDriverAgent.xcodeproj)")"

Step 3. Navigate to WebDriverAgent project path in terminal and run below command to setup the project:
mkdir -p Resources/WebDriverAgent.bundle

Step 4. Open WebDriverAgent.xcodeproj in Xcode. For both the WebDriverAgentLib and WebDriverAgentRunner targets, select "Automatically manage signing" in the "General" tab, and then select your Development Team. This should also auto select Signing Certificate. 

Step 5. Manually change the bundle id for the target by going into the "Build Settings" tab, and changing the "Product Bundle Identifier" from com.facebook.WebDriverAgentRunner to 
something that Xcode will accept (something really unique!)

Step 6. Command to build the project:
xcodebuild -project WebDriverAgent.xcodeproj -scheme WebDriverAgentRunner -destination 'id=<udid>' test -allowProvisioningUpdates

Note: If mirroring device using Quick Time Player at the same, there could be port conflict. In that case, start Appium server with a different port of WebDriverAgent.
appium server --driver-xcuitest-webdriveragent-port 8101

Step 7. Create session with app using Appium Inspector pointing to CLI server
