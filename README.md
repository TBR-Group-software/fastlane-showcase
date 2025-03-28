# Fastlane iOS TestFlight Deployment - Example Setup

<p align="center">
  <img src="https://fastlane.tools/assets/img/fastlane.png" alt="Fastlane Logo" width="250">
</p>

This repository serves as a reference implementation for automating iOS app distribution to TestFlight using Fastlane. It demonstrates best practices and configuration patterns that you can adapt for your own iOS projects.

## Overview

This example showcases:

- ✅ Certificate and provisioning profile management with Match
- ✅ App Store Connect API integration for seamless deployments
- ✅ Automated build number incrementation
- ✅ TestFlight deployment automation
- ✅ Environment variable management for secure credential storage

## Repository Structure

```
fastlane/
├── Appfile               # App-specific configuration
├── Fastfile              # Automation lanes (core implementation)
├── Matchfile             # Certificate management configuration
├── .env                  # Environment variables for credentials
```

## Key Files Explained

### Fastfile

The Fastfile is the heart of the implementation, containing two main lanes:

1. **`create_certificates_and_profiles`**: Sets up all necessary certificates for development, ad-hoc, and App Store distribution
2. **`upload_to_testflight_function`**: Handles the entire TestFlight deployment process

```ruby
desc "Build and upload to TestFlight"
lane :upload_to_testflight_function do
  # Setup for CI environments
  setup_ci

  # Configure API access
  app_store_connect_api_key(...)

  # Fetch certificates
  sync_code_signing(...)

  # Update code signing settings
  update_code_signing_settings(...)

  # Increment build number
  increment_build_number(...)

  # Build the app
  build_app(...)

  # Upload to TestFlight
  upload_to_testflight(...)
end
```

### Matchfile

The Matchfile demonstrates how to configure Match for certificate management:

```ruby
git_url("YOUR-GITHUB-REPO-URL")
storage_mode("git")
type("development")
```

In your own implementation, you'll need to:

- Create a private repository for certificate storage
- Update the `git_url` with your repository

### Environment Variables

The repository demonstrates how a .env file should be configured. For your implementation, you'll need to set up credentials like:

```
ASC_KEY_ID="ABC123DEFG"
ASC_ISSUER_ID="aaaaa-bbbbb-ccccc-ddddd"
ASC_KEY_PATH="/path/to/AuthKey_ABC123DEFG.p8"
```

## Implementing in Your Own Project

To adapt this example for your own iOS app:

1. **Install Fastlane** in your project:

   ```bash
   cd ios  # Navigate to your iOS project directory
   gem install fastlane
   ```

2. **Copy the key files** from this repository:

   - Fastfile
   - Matchfile
   - Appfile

3. **Modify the configuration** for your specific app:

   - Update bundle identifiers
   - Set your team ID
   - Configure your build schemes

4. **Set up App Store Connect API access**:

   - Create API keys in [App Store Connect](https://appstoreconnect.apple.com/)
   - Create a `.env`

5. **Set up a private certificate repository** for Match:

   - Create a private GitHub repository
   - Update the Matchfile with your repository URL

6. **Generate certificates** with:

   ```bash
   fastlane create_certificates_and_profiles
   ```

7. **Run your first deployment** with:
   ```bash
   fastlane upload_to_testflight_function
   ```

## Common Challenges & Solutions

### Certificate Management

If your team encounters certificate issues, have them run:

```bash
fastlane match nuke distribution
fastlane match appstore
```

### Build Number Management

The example automatically increments build numbers by fetching the latest from TestFlight:

```ruby
increment_build_number(build_number: latest_testflight_build_number + 1)
```

## Resources

- [Official Fastlane Documentation](https://docs.fastlane.tools/)
- [Match for Certificate Management](https://docs.fastlane.tools/actions/match/)

## License

This repository is licensed under the GNU General Public License v3.0 (GPL-3.0). This is a strong copyleft license that requires anyone who distributes your code or derivative works to make the source available under the same terms. For the full license text, see the LICENSE file in the repository.

---

Developed by TBR Group as a reference implementation for the iOS community. Visit [TBR Group](https://tbrgroup.software/blog/) for more resources.
