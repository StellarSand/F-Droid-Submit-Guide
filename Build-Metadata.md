# 3. Build Metadata
- Create an account on [GitLab](https://about.gitlab.com).
- Fork the [fdroiddata](https://gitlab.com/fdroid/fdroiddata) repo.
- Create a new branch named after your app’s name or its package name.
- In the newly created branch, go to the `metadata` directory & create a new file named `<your app's package name>.yml`. Example: `com.myapp.yml`

You can read more about the build metadata [here](https://f-droid.org/en/docs/Build_Metadata_Reference/).

Before proceeding further, decide whether you want reproducible builds or not.
<br>A benefit of reproducible builds is that users can upgrade to a new version of your app irrespective of where they previously installed it from. Example: Users can install the apk from GitHub and upgrade later to a newer version using F-Droid & vice-versa.



## Reproducible builds
- Assuming that when creating a tagged release earlier, the tag was named with a `v` (example: `v1.0.0`) and the apk was named `MyApp_v1.0.0`.
- In the root directory of your app, open the `local.properties` file.
- Copy the `sdk.dir` value. Example: `/home/JohnDoe/Android-SDK`.
- Go to `/home/JohnDoe/Android-Sdk/build-tools/` & check the latest `build-tools` version installed. Example: `34.0.0`.
- Open a terminal.
- Navigate to the directory:
  ```bash
    cd /home/JohnDoe/Android-Sdk/build-tools/34.0.0
  ```
- Use `apksigner` to get the SHA-256 digest of your signing certificate:
  ```bash
    ./apksigner verify --print-certs /path/to/the/generated_signed_apk
  ```
  Example:
  ```bash
  ./apksigner verify --print-certs /home/JohnDoe/AndroidStudioProjects/MyApp/app/release/MyApp_v1.0.0-release.apk
  ```
- Copy the `Signer #1 certificate SHA-256 digest` value.
- Type the metadata in the build metadata file created earlier (`com.myapp.yml`).
  <br>Here's an example for reproducible builds:
  ```yml
  Categories:
    - Connectivity
  License: GPL-3.0-only
  AuthorName: John Doe
  SourceCode: https://github.com/JohnDoe/MyApp
  IssueTracker: https://github.com/JohnDoe/MyApp/issues
  Translation: https://github.com/JohnDoe/MyApp/blob/main/CONTRIBUTING.md
  Changelog: https://github.com/JohnDoe/MyApp/releases
  
  AutoName: App name
  
  RepoType: git
  Repo: https://github.com/JohnDoe/MyApp.git
  Binaries: https://github.com/JohnDoe/MyApp/releases/download/v%v/MyApp_v%v.apk
  
  Builds:
    - versionName: 1.0.0
      versionCode: 100
      commit: commit number like 0bd29264d182cf456cf80d490abf285dfcb12f92
      subdir: app
      gradle:
        - yes
  
  AllowedAPKSigningKeys: SHA-256 digest of your signing certificate
  
  AutoUpdateMode: Version
  UpdateCheckMode: Tags
  CurrentVersion: 1.0.0
  CurrentVersionCode: 100
  ```
- Commit the changes



## Non-reproducible builds
- Assuming that when creating a tagged release earlier, the tag was named with a `v` (example: `v1.0.0`) and the apk was named `MyApp_v1.0.0`.
- Type the metadata in the build metadata file created earlier (`com.myapp.yml`).
  <br>Here's an example for non-reproducible builds:
  ```yml
  Categories:
    - Connectivity
  License: GPL-3.0-only
  AuthorName: John Doe
  SourceCode: https://github.com/JohnDoe/MyApp
  IssueTracker: https://github.com/JohnDoe/MyApp/issues
  Translation: https://github.com/JohnDoe/MyApp/blob/main/CONTRIBUTING.md
  Changelog: https://github.com/JohnDoe/MyApp/releases
  
  AutoName: App name
  
  RepoType: git
  Repo: https://github.com/JohnDoe/MyApp.git
  
  Builds:
    - versionName: 1.0.0
      versionCode: 100
      commit: commit number like 0bd29264d182cf456cf80d490abf285dfcb12f92
      subdir: app
      sudo:
        - apt-get update
        - apt-get install -y openjdk-17-jdk-headless
        - update-java-alternatives -a
      gradle:
        - yes
  
  AutoUpdateMode: Version
  UpdateCheckMode: Tags
  CurrentVersion: 1.0.0
  CurrentVersionCode: 100
  ```
- Commit the changes.

[<img align="left" src="https://img.shields.io/static/v1?logo=data:image/svg%2bxml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGhlaWdodD0iNDhweCIgdmlld0JveD0iMCAtOTYwIDk2MCA5NjAiIHdpZHRoPSI0OHB4IiBmaWxsPSIjRkFGQUZBIj48cGF0aCBkPSJtMzMwLTQ0NCAxNzYuMjEgMTc2LjIxUTUxNy0yNTcgNTE3LTI0Mi41dC0xMSAyNS45OFE0OTUtMjA2IDQ4MC41LTIwNnQtMjUuMzEtMTAuODJMMjE3LjQtNDU0LjkxcS01LjQtNS40MS03LjktMTEuNzItMi41LTYuMzEtMi41LTEzLjUzIDAtNy4yMSAyLjUtMTMuNTNRMjEyLTUwMCAyMTctNTA1bDIzOC0yMzhxMTEtMTEgMjUuNjctMTEgMTQuNjYgMCAyNS4zMyAxMSAxMSAxMC42NyAxMSAyNS4zMyAwIDE0LjY3LTEwLjg0IDI1LjUxTDMzMC01MTZoNDAyLjAycTE1LjI5IDAgMjUuNjQgMTAuMjlRNzY4LTQ5NS40MiA3NjgtNDgwLjIxdC0xMC4zNCAyNS43MVE3NDcuMzEtNDQ0IDczMi4wMi00NDRIMzMwWiIvPjwvc3ZnPgo=&label=&message=Previous&color=blue&style=for-the-badge" alt="Previous">](https://github.com/StellarSand/F-Droid-Submit-Guide/blob/main/Release-Signed-Build.md)
[<img align="right" src="https://img.shields.io/static/v1?logo=data:image/svg%2bxml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGhlaWdodD0iNDhweCIgdmlld0JveD0iMCAtOTYwIDk2MCA5NjAiIHdpZHRoPSI0OHB4IiBmaWxsPSIjRkFGQUZBIj48cGF0aCBkPSJNNjg2LTQ1MEgxOTBxLTEzIDAtMjEuNS04LjVUMTYwLTQ4MHEwLTEzIDguNS0yMS41VDE5MC01MTBoNDk2TDQ1OS03MzdxLTktOS05LTIxdDktMjFxOS05IDIxLTl0MjEgOWwyNzggMjc4cTUgNSA3IDEwdDIgMTFxMCA2LTIgMTF0LTcgMTBMNTAxLTE4MXEtOSA5LTIxIDl0LTIxLTlxLTktOS05LTIxdDktMjFsMjI3LTIyN1oiLz48L3N2Zz4=&label=&message=Next&color=blue&style=for-the-badge" alt="Next">](https://github.com/StellarSand/F-Droid-Submit-Guide/blob/main/Merge-Request.md)