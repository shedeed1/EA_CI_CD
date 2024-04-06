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
| `APP_BUILD_NUMBER`          | (Optional) Application build number         | Specify app build number, if not specified it will increment the latest version code on Firebase/Play Store  |
| `APP_BUILD_NAME`        | (Optional) Android build name      | Specify app build name, if not specified, it will use the build.gradle build name                             |
| `ANDROID_PACKAGE_NAME`   | (Required) Android package name | Format: com.abc.xyz, can be found in app build.gradle file                     |
| `ANDROID_FLAVOR`        | (Required) Android build flavor for local builds            | Can be staging or production                         |
| `ANDROID_SKIP_SIGNING`    | (Optional) Whether to skip app signing or not |    | Can be true to skip signing or false to not to skip
| `ANDROID_STORE_FILE`           | (Required) Android Keystore file path | [Android Signing Docs](https://developer.android.com/studio/publish/app-signing)                  |
| `ANDROID_STORE_PASSWORD`                 | (Required) Android keystore password | [Android Signing Docs](https://developer.android.com/studio/publish/app-signing)                  |
| `ANDROID_KEY_ALIAS`              | (Required) Android Key Alias       | [Android Signing Docs](https://developer.android.com/studio/publish/app-signing)                                                                                                       |
| `ANDROID_KEY_PASSWORD` | (Required) Android Key Password | [Android Signing Docs](https://developer.android.com/studio/publish/app-signing) |
| `GOOGLE_JSON_FILE`            | (Required) Google service credential key json file path | [Fastlane Docs](https://docs.fastlane.tools/getting-started/android/setup/)                                           |
| `FIREBASE_APP_ID`           | (Required) Firebase App ID                                  | Can be fetched from project settings in Firebase console |
| `FIREBASE_GROUPS`           | (Optional) Firebase Test Groups                              | Example: "qa-team, trusted-testers" |
| `FIREBASE_RELEASE_NOTES`    | (Optional) Firebase build release notes                              | Example: "New exciting features!" |


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
4. **Create .circleci_flavor in your project root folder and write "staging" or "production" in the file based on your desired flavor**
5. **Navigate to CircleCI project settings then "Environment Variables" from the left panel**
6. **Make sure you enter all of the data in the below table as environment variables (including variables for staging environment):**

| Input                       | Description                       | Reference                                                                                              |
|-----------------------------|-----------------------------------|--------------------------------------------------------------------------------------------------------|
| `APP_BUILD_NUMBER`          | (Optional) Application build number         | Specify app build number, if not specified it will increment the latest version code on Firebase/Play Store  |
| `APP_BUILD_NUMBER_STAGING`          | (Optional) Application build number         | Specify app build number, if not specified it will increment the latest version code on Firebase/Play Store  |
| `APP_BUILD_NAME`        | (Optional) Android build name      | Specify app build name, if not specified, it will use the build.gradle build name    |
| `APP_BUILD_NAME_STAGING`        | (Optional) Android build name      | Specify app build name, if not specified, it will use the build.gradle build name    |
| `ANDROID_PACKAGE_NAME`   | (Required) Android package name | Format: com.abc.xyz, can be found in app build.gradle file                     |
| `ANDROID_PACKAGE_NAME_STAGING`   | (Required) Android package name | Format: com.abc.xyz, can be found in app build.gradle file                     |
| `ANDROID_SKIP_SIGNING`    | (Optional) Whether to skip app signing or not |    |
| `ANDROID_SKIP_SIGNING_STAGING`    | (Optional) Whether to skip app signing or not |    |
| `ANDROID_KEYSTORE_BASE64`           | (Required) Android Keystore file encoded as base64 string | [Android Signing Docs](https://developer.android.com/studio/publish/app-signing)                  |
| `ANDROID_KEYSTORE_BASE64_STAGING`           | (Required) Android Keystore file encoded as base64 string | [Android Signing Docs](https://developer.android.com/studio/publish/app-signing)                  |
| `ANDROID_STORE_PASSWORD`                 | (Required) Android keystore password | [Android Signing Docs](https://developer.android.com/studio/publish/app-signing)                  |
| `ANDROID_STORE_PASSWORD_STAGING`                 | (Required) Android keystore password | [Android Signing Docs](https://developer.android.com/studio/publish/app-signing)                  |
| `ANDROID_KEY_ALIAS`              | (Required) Android Key Alias       | [Android Signing Docs](https://developer.android.com/studio/publish/app-signing)                                                                                                       |
| `ANDROID_KEY_ALIAS_STAGING`              | (Required) Android Key Alias       | [Android Signing Docs](https://developer.android.com/studio/publish/app-signing)                                                                                                       |
| `ANDROID_KEY_PASSWORD` | (Required) Android Key Password | [Android Signing Docs](https://developer.android.com/studio/publish/app-signing) |
| `ANDROID_KEY_PASSWORD_STAGING` | (Required) Android Key Password | [Android Signing Docs](https://developer.android.com/studio/publish/app-signing) |
| `GOOGLE_JSON_KEY_BASE64`            | (Required) Google service credential key json file encoded as base64 | [Fastlane Docs](https://docs.fastlane.tools/getting-started/android/setup/)                                           |
| `FIREBASE_APP_ID`           | (Required) Firebase App ID                                  | Can be fetched from project settings in Firebase console |
| `FIREBASE_APP_ID_STAGING`           | (Required) Firebase App ID                                  | Can be fetched from project settings in Firebase console |
| `FIREBASE_GROU
PS`           | (Optional) Firebase Test Groups                              | Example: "qa-team, trusted-testers" |

7. **You'll have something similar to the below screenshot:**
<img width="750" alt="image" src="https://github.com/shedeed1/EA_CI_CD/assets/26841936/75c7e14f-a6e1-4d7e-a798-2b7df6c5402c">

8. **By default, the pipeline is triggered on every commit, staging builds go to Firebase App Distribution while production builds go to Google Play**
