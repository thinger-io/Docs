# USER ACCOUNTS

## Sign-in Methods

This section displays all the methods available to access your account. To access your security settings, navigate to **Account** → **Security**.

### Email & Password

Your primary email address is displayed along with its verification status. You can set or update your password from this section:

* **Set Password**: If you signed up using a federated identity provider (like Google or Microsoft), you may not have a password set. You can create one to have an alternative sign-in method.
* **Update Password**: If you already have a password, you can change it by entering your current password followed by the new one.

Password requirements are configured by your platform administrator and typically require a minimum length of 6-8 characters.

### Passkeys

Passkeys provide a modern, passwordless way to sign in to your account using biometric authentication (fingerprint, face recognition) or a hardware security key. Passkeys are:

* **More secure** than passwords - they can't be phished or stolen
* **Easier to use** - no need to remember complex passwords
* **Cross-platform** - can sync across your devices (depending on your platform)

#### Adding a Passkey

1. In the **Passkeys for Sign-in** section, enter a descriptive name for your passkey (e.g., "MacBook Pro", "iPhone", "Windows PC")
2. Click **Add Passkey**
3. Your browser will prompt you to authenticate using:
   * Biometric authentication (Touch ID, Face ID, Windows Hello)
   * A hardware security key
   * Your device PIN
4. Once verified, the passkey is registered and appears in your list

#### Managing Passkeys

Your registered passkeys are displayed in a list showing:

* **Name**: The identifier you assigned
* **Authenticator type**: The type of authenticator used (when available)
* **Created**: When the passkey was registered
* **Last used**: When you last signed in with this passkey

To remove a passkey, click the delete icon next to it and confirm the action.

#### Signing in with a Passkey

When passkey login is enabled on your platform:

1. On the login page, click **Sign in with Passkey**
2. Your browser will prompt you to select and authenticate with a registered passkey
3. After successful authentication, you're signed in directly

> **Note**: Passkeys require a compatible browser and device. If your browser doesn't support WebAuthn, the passkey options won't be visible.

### Connected Accounts

If your platform has federated identity providers configured (such as Google, Microsoft, Auth0, or others), this section displays your linked accounts.

For each connected account, you can see:

* **Provider**: The identity provider (with logo)
* **Email**: The email address associated with that provider
* **Linked date**: When you connected the account

#### Unlinking an Account

You can disconnect a federated identity by clicking the unlink button. However, to maintain access to your account, you must have at least one of the following:

* A password set on your account
* Another connected identity provider

If you only have one connected account and no password, you'll need to set a password before you can unlink it.

***

## Two-Factor Authentication (2FA)

Two-factor authentication adds an extra layer of security to your account. After entering your password, you'll need to provide a second form of verification.

Your platform supports two types of second factors:

### Authenticator App (TOTP)

Time-based One-Time Passwords (TOTP) work with authenticator apps like:

* Google Authenticator
* Microsoft Authenticator
* Authy
* 1Password
* Any TOTP-compatible app

#### Setting up an Authenticator App

1. Click **Enable Authenticator App**
2. **Step 1 - Scan QR Code**:
   * Open your authenticator app and scan the displayed QR code
   * Alternatively, manually enter the secret key shown below the QR code
3. **Step 2 - Verify Code**:
   * Enter the 6-digit code displayed in your authenticator app
   * Click **Verify**
4. **Step 3 - Save Backup Codes**:
   * You'll receive 10 single-use backup codes
   * **Save these codes in a safe place** - they're your recovery option if you lose access to your authenticator app
   * Use the **Copy** button to copy all codes to your clipboard
   * Use the **Print** button to print a formatted page with your codes
5. Click **Done** to complete setup

Once enabled, you'll see the status showing "Authenticator app is enabled" along with the number of remaining backup codes.

#### Disabling the Authenticator App

Click **Disable Authenticator App** and confirm the action. This will remove TOTP as a second factor and invalidate all backup codes.

### Security Keys (WebAuthn)

Hardware security keys provide phishing-resistant two-factor authentication. Compatible devices include:

* YubiKey
* Google Titan Security Key
* Feitian keys
* Any FIDO2/WebAuthn compatible security key

#### Adding a Security Key

1. In the **Security Keys** section, enter a name for your key (e.g., "YubiKey 5", "Titan Key")
2. Click **Add Security Key**
3. When prompted by your browser:
   * Insert your security key (if USB)
   * Tap or activate the key when it blinks
4. The key is registered and appears in your list

#### Managing Security Keys

Your registered security keys are displayed showing:

* **Name**: The identifier you assigned
* **Authenticator type**: The detected key type
* **Created**: Registration date
* **Last used**: When you last used this key for authentication

You can register multiple security keys for redundancy. To remove a key, click the delete icon and confirm.

### Backup Codes

Backup codes are generated when you enable the authenticator app. Each code:

* Can only be used **once**
* Is 8 characters long
* Should be stored securely offline

#### Using a Backup Code

During login, when prompted for your two-factor code:

1. Click **Use a backup code instead**
2. Enter one of your backup codes
3. The code is consumed and can't be used again

#### Checking Remaining Codes

Your security settings show how many backup codes you have remaining. If you're running low, consider disabling and re-enabling the authenticator app to generate a fresh set of 10 codes.

***

## Authentication Flow with 2FA

When two-factor authentication is enabled, your login process becomes:

1. Enter your username and password (or use a federated identity)
2. You're prompted for your second factor
3. Choose your verification method:
   * **Authenticator App**: Enter the 6-digit code from your app
   * **Security Key**: Insert and activate your hardware key
   * **Backup Code**: Enter an 8-character backup code
4. After successful verification, you're signed in

If you have multiple 2FA methods configured, you can switch between them using the **Try another method** link.

> **Note**: Signing in with a Passkey bypasses password and 2FA entirely, as passkeys already provide strong authentication.

***

## Best Practices

1. **Enable at least one form of 2FA** to protect your account from unauthorized access
2. **Register multiple passkeys or security keys** on different devices for redundancy
3. **Store backup codes securely** - consider a password manager or a physical safe
4. **Don't share your backup codes** - treat them like passwords
5. **Remove old or unused credentials** - if you no longer use a device, remove its passkey or security key
6. **Keep your authenticator app backed up** - some apps offer cloud sync for recovery
