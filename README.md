
# Appium and Selenium version compatibility
To run mobile tests, set Appium-Python-Client to 1.1.0, selenium to 3.141.0 and Appium-server to 1.22.2

## Prerequisites

Before you begin, ensure that you have the following prerequisites:

- [ ] Node.js installed on your machine. You can download it from [https://nodejs.org/](https://nodejs.org/).
- [ ] Appium installed globally. You can install it using the command: `npm install -g appium@1.22`.
- [ ] Java Development Kit (JDK) installed. Download the latest JDK from [https://www.oracle.com/java/technologies/javase-downloads.html](https://www.oracle.com/java/technologies/javase-downloads.html).

## Appium Installation on macOS and Windows

Appium does not support testing iOS applications on Windows directly. For iOS testing, use a macOS machine.

### Android Configuration on macOS and Windows

1. Install Android Studio by following the instructions at [https://developer.android.com/studio/install](https://developer.android.com/studio/install).

2. Set the Android Home environment variable:
   ```bash
   export ANDROID_HOME=$HOME/Library/Android/sdk
   export PATH=$PATH:$ANDROID_HOME/emulator
   export PATH=$PATH:$ANDROID_HOME/tools
   export PATH=$PATH:$ANDROID_HOME/tools/bin
   export PATH=$PATH:$ANDROID_HOME/platform-tools
   ```
   
3. Launch Android Studio and install the necessary SDK packages.
4. To get device id:
   ```bash
   adb devices
   ```
5. To get device name:
   Check device name in device settings


### Configuring for iOS on macOS

1. Install Xcode from the App Store:
   ```bash
   xcode-select --install
   brew install carthage
   npm install -g ios-deploy
   ```
2. Install the Appium Xcode WebDriverAgent:
   1. **Locate WebDriverAgent. If you installed Appium via Node.js:**
        ```
        /usr/local/lib/node_modules/appium/node_modules/appium-webdriveragent
        ```
   2. **Navigate to the Directory:**
      In the Terminal window, go to the directory specified above.
   3. **Run the Installation Commands:**
      Execute the following commands in the Terminal:
      ```bash
      mkdir -p Resources/WebDriverAgent.bundle
      ./Scripts/bootstrap.sh -d
      ```
   4. **Open WebDriverAgent.xcodeproj in Xcode:**
      1. Locate the WebDriverAgent.xcodeproj project in one of the specified directories.
      2. Open the project in Xcode and Configure Targets in Xcode:
      For the WebDriverAgentLib and WebDriverAgentRunner targets, go to the General tab.
      Select the "Automatically manage signing" checkbox, and then choose your development team and build project
      3. Build the Project:
      ```bash
      xcodebuild -project WebDriverAgent.xcodeproj -scheme WebDriverAgentRunner -destination 'id=udid' test
      ```
3. Identify Connected Devices: If you have physical devices connected to your Appium instance, use the following command to output the list of device IDs.
Copy the ID of the desired device for later use in the "capabilities" section of mobile tests.
      ```bash
      ios-deploy -c
      ```


### Set up python environment for autotests

1. Make sure you have python installed on your machine by typing in console "python --version" 
   (python 3.12 was used for test development)
2. Activate VirtualEnv:
  ```bash
  source venv/bin/activate
  ```
3. Install requirements.txt
  ```bash
  pip install -r requirements.txt
  ```
4. Install Allure for reporting. Instruction can be found here: https://docs.qameta.io/allure-report/
5. Install PyTest as default runner(instruction for PyCharm IDE )
- PyCharm - Preferences - Tools - Python integrated tools - default test runner: pytest


### Run tests
- To run all tests execute next(driver_value, device_name and udid must be specified in pytest_addoption
  func in conftest.py):
  ```bash
  pytest mobile
  ```
- To run specific tests execute next(driver_value, device_name and udid must be specified in pytest_addoption
  func in conftest.py):
  ```bash
  pytest mobile/tests/tests/test_search.py::TestSearch::test_search_results
  ```
- To run tests with specific device or emulator run next:
  - for Android:
  ```bash
  pytest mobile --driver_value android_driver --device_name Pixel_3a_API_34_extension_level_7_arm64-v8a --udid emulator-5554 --platform_version 14.0
  ```
  - for iOS:
  ```bash
  pytest mobile --driver_value ios_driver --device_name iPhone_15_Pro  --platform_version 17.0
  ```
- To run test with allure report execute next (driver_value, device_name and udid must be specified in pytest_addoption
  func in conftest.py):
  ```bash
  pytest mobile --alluredir=reports
  ```
- To serve allure report execute next:
  ```bash 
  allure serve reports
  ```

