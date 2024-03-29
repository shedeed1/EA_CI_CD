## How to Setup EA CI/CD Fastlane script locally

1. **Navigate to your project root folder**

2. **Setup Fastlane:**
   - Install fastlane using these commands:
      ```
      brew install fastlane
      ```
   - Copy Gemfile to your project root folder.
   - Run the following command:
     ```
     bundle update
     ```
   - Copy fastlane folder and paste it in your project root folder

3. **Configure Environment variables**
   - Create `.env.production` & `.env.staging` files in your project root folder.
   - Modify the environment variables to match your app **Note** If you leave `APP_BUILD_NUMBER` empty, the script will fetch the latest version code from Firebase or Google Play Store and increment that.

4. **Update App gradle file to ensure proper version code injection:**
   - Navigate to your app build.gradle.kts and make sure you have the following code for your `versionCode` and `versionName`
      ```
      versionCode = project.findProperty("versionCode")?.toString()?.toInt() ?: 1
      versionName = project.findProperty("versionName")?.toString() ?: "1.0"
      ```

## Environment variables documentation

| Input                       | Description                       | Reference                                                                                              |
|-----------------------------|-----------------------------------|--------------------------------------------------------------------------------------------------------|
| `APP_BUILD_NUMBER`          | Application build number         | Specify app build number, if not specified it will increment the latest version code on Firebase/Play Store  |
| `APP_BUILD_NAME`        | Android build name      |                              |
| `ANDROID_PACKAGE_NAME`   | Android package name | Format: com.abc.xyz, can be found in app build.gradle file                     |
| `ANDROID_FLAVOR`        | Android build flavor            | Can be staging or production                         |
| `ANDROID_SKIP_SIGNING`    | Whether to skip app signing or not |    |
| `ANDROID_STORE_FILE`           | Android Keystore file path | [Android Signing Docs](https://developer.android.com/studio/publish/app-signing)                  |
| `ANDROID_STORE_PASSWORD`                 | Android keystore password | [Android Signing Docs](https://developer.android.com/studio/publish/app-signing)                  |
| `ANDROID_KEY_ALIAS`              | Android Key Alias       | [Android Signing Docs](https://developer.android.com/studio/publish/app-signing)                                                                                                       |
| `ANDROID_KEY_PASSWORD` | Android Key Password | [Android Signing Docs](https://developer.android.com/studio/publish/app-signing) |
| `GOOGLE_JSON_FILE`            | Google service credential key json file path | [Fastlane Docs](https://docs.fastlane.tools/getting-started/android/setup/)                                           |
| `FIREBASE_APP_ID`           | Firebase App ID                                  | Can be fetched from project settings in Firebase console |
| `FIREBASE_GROUPS`           | Firebase Test Groups                              | Example: "qa-team, trusted-testers" |
| `FIREBASE_RELEASE_NOTES`    | Firebase build release notes                              | Example: "New exciting features!" |


## How to Run the Fastlane script

1. **Navigate to your project root folder.**
2. **Run the following command if you want to push to Firebase App Distribution using staging environment.**
  ```
  fastlane android deploy_firebase --env staging
  ```
  Alternatively, you can choose to run the production environment
  ```
  fastlane android deploy_firebase --env production
  ```
3. **Run the following command if you want to push to Google Play using production environment.**
  ```
  fastlane android deploy_google_play --env production
  ```
  Alternatively, you can choose to run the staging environment
  ```
  fastlane android deploy_google_play --env staging
  ```

## How to Setup EA CI/CD on CircleCI

1. **Once you've made sure that the script is working fine locally**
2. **Navigate to CircleCI web app and create a new project**
   - Navigate to "Projects" and select "Create Project"
    <img width="923" alt="image" src="https://github.com/shedeed1/EA_CI_CD/assets/26841936/b6c0cae5-6738-4660-80db-cbd9bc059da0">
   - Make sure you follow the instructions so that your Github repo is connected to your CircleCI project

3. **Make sure you place the config.yml file in your .circleci folder**
4. **Navigate to CircleCI project settings then "Environment Variables" from the left panel**
5. **Make sure you enter all of the data in the below table as environment variables:**

| Input                       | Description                       | Reference                                                                                              |
|-----------------------------|-----------------------------------|--------------------------------------------------------------------------------------------------------|
| `APP_BUILD_NUMBER`          | (Optional) Application build number         | Specify app build number, if not specified it will increment the latest version code on Firebase/Play Store  |
| `APP_BUILD_NAME`        | (Required) Android build name      |                              |
| `ANDROID_PACKAGE_NAME`   | (Required) Android package name | Format: com.abc.xyz, can be found in app build.gradle file                     |
| `ANDROID_FLAVOR`        | (Required) Android build flavor            | Can be staging or production                         |
| `ANDROID_SKIP_SIGNING`    | (Optional) Whether to skip app signing or not |    |
| `ANDROID_KEYSTORE_BASE64`           | (Required) Android Keystore file encoded as base64 string | [Android Signing Docs](https://developer.android.com/studio/publish/app-signing)                  |
| `ANDROID_STORE_PASSWORD`                 | (Required) Android keystore password | [Android Signing Docs](https://developer.android.com/studio/publish/app-signing)                  |
| `ANDROID_KEY_ALIAS`              | (Required) Android Key Alias       | [Android Signing Docs](https://developer.android.com/studio/publish/app-signing)                                                                                                       |
| `ANDROID_KEY_PASSWORD` | (Required) Android Key Password | [Android Signing Docs](https://developer.android.com/studio/publish/app-signing) |
| `GOOGLE_JSON_KEY_BASE64`            | (Required) Google service credential key json file encoded as base64 | [Fastlane Docs](https://docs.fastlane.tools/getting-started/android/setup/)                                           |
| `FIREBASE_APP_ID`           | (Required) Firebase App ID                                  | Can be fetched from project settings in Firebase console |
| `FIREBASE_GROUPS`           | (Optional) Firebase Test Groups                              | Example: "qa-team, trusted-testers" |
| `FIREBASE_RELEASE_NOTES`    | (Optional) Firebase build release notes, this defaults to the latest Github commit | Example: "New exciting features!" |

6. **You'll have something similar to the below screenshot:**
<img width="998" alt="image" src="https://github.com/shedeed1/EA_CI_CD/assets/26841936/1a180bed-86a9-4c2c-889c-bf73bd56a8f6">

7. **By default, the pipeline is triggered on every commit**
