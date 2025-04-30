# **iOS TestFlight Deployment via Fastlane & GitHub Actions**

This repository provides a reference implementation for deploying iOS applications to **TestFlight** using **Fastlane** and **GitHub Actions**. It includes automated certificate management, API-based authentication, and secure secret handling for CI environments.

---

## **📚 Table of Contents**

- [Features](#-features)
- [Directory Structure](#-directory-structure)
- [GitHub Actions Workflow](#-github-actions-workflow)
- [Required Secrets](#-required-secrets)
- [How to Run the GitHub Action Manually](#-how-to-run-the-github-action-manually)
- [Integrate into Your Own Project](#️-integrate-into-your-own-project)
- [Resources](#-resources)
- [License](#-license)

---

## **🔧 Features**

- ✅ Automated deployment to TestFlight using Fastlane
- ✅ Secure handling of App Store Connect credentials
- ✅ CI-ready with GitHub Actions
- ✅ Certificate & provisioning profile management via match
- ✅ Build number auto-incrementation using latest TestFlight build

---

## **📁 Directory Structure**

```
.github/
└── workflows/
    └── ios-testflight.yml      # GitHub Actions CI pipeline

ios/
└── fastlane/
    ├── Appfile                 # App identifier & team ID
    ├── Fastfile                # Deployment lanes (Fastlane logic)
    ├── Matchfile               # match configuration for code signing
    └── .env                    # API credentials (excluded from VCS)
```

---

## **🚀 GitHub Actions Workflow**

The GitHub Actions workflow performs the following steps:

1. **Check out code** from the default branch
2. **Install dependencies**: Flutter, Ruby gems, and SSH agent
3. **Decode App Store Connect API key** from a base64 string
4. **Execute Fastlane** lane to:
   - Configure App Store Connect access
   - Sync code signing assets using match
   - Update Xcode code signing settings
   - Increment the build number automatically
   - Build the .ipa file
   - Upload to TestFlight

> The Fastlane logic is encapsulated in a custom lane called upload_to_testflight_function, defined in ios/fastlane/Fastfile.

---

## **🔐 Required Secrets**

These secrets must be configured in your repository settings under **Settings → Secrets and variables → Actions**:

| **Secret Key**  | **Description**                                       |
| --------------- | ----------------------------------------------------- |
| SSH_PRIVATE_KEY | SSH key to access the private match certificates repo |
| ASC_KEY_BASE64  | Base64-encoded App Store Connect API .p8 key          |
| ASC_KEY_ID      | App Store Connect API Key ID                          |
| ASC_ISSUER_ID   | App Store Connect API Issuer ID                       |
| MATCH_PASSWORD  | Password used to decrypt the certificates             |

---

## **🧪 How to Run the GitHub Action Manually**

> ⚠️ The workflow must be committed to the
>
> **default branch**

To run the deployment manually:

1. Go to the **Actions** tab in your GitHub repository
2. Select the workflow titled iOS TestFlight Deployment
3. Click **Run workflow** in the top-right
4. Confirm and monitor the run

---

## **🛠️ Integrate into Your Own Project**

To adapt this setup for your iOS app:

1. **Copy the workflow** file to .github/workflows/ios-testflight.yml
2. **Copy the Fastlane config** from ios/fastlane/ into your own iOS directory
3. Update the following:
   - Bundle ID in Appfile
   - Team ID in Appfile
   - Match certificate repository URL in Matchfile
   - Build scheme in Fastfile
4. Add the required GitHub secrets listed above
5. Generate your initial code signing assets:

```
cd ios
fastlane match appstore
```

6. Run your first deployment:

```
bundle exec fastlane upload_to_testflight_function
```

---

## **📚 Resources**

- 📘 [Fastlane Documentation](https://docs.fastlane.tools/)
- 🔐 [Match for Certificate Management](https://docs.fastlane.tools/actions/match/)
- 🚀 [App Store Connect API](https://developer.apple.com/documentation/appstoreconnectapi)
- 🧪 [GitHub Actions Documentation](https://docs.github.com/en/actions)

---

## **📄 License**

This repository is licensed under the [GNU General Public License v3.0 (GPL-3.0)].

---

> Built and maintained by
>
> **TBR Group**
>
> [tbrgroup.software](https://tbrgroup.software/blog/)
