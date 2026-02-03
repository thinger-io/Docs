# IDENTITY PROVIDERS

## Identity Providers

Identity Providers allow users to sign in to your platform using external authentication services like Google, Microsoft, Auth0, Okta, PingOne, or any OpenID Connect (OIDC) compliant provider. This enables Single Sign-On (SSO) and simplifies user management by delegating authentication to trusted third-party services.

To manage identity providers, navigate to **Identity Providers** in the admin console.

### Creating an Identity Provider

Click **Add Identity Provider** to configure a new provider. The configuration is organized into several sections:

#### Basic Information

* **Identifier**: A unique lowercase identifier for this provider (e.g., `google`, `azure-ad`, `okta-prod`). This identifier is used internally and in URLs.
* **Name**: The display name shown to users on the login page (e.g., "Google", "Company SSO").
* **Description**: Optional notes for administrators about this provider's purpose or configuration.
* **Type**: The authentication protocol. Currently, only **OIDC** (OpenID Connect) is supported.
* **Enabled**: Toggle to enable or disable the provider. Disabled providers won't appear on the login page.

#### OIDC Configuration

This section contains the core settings for connecting to your identity provider.

**Domain**

The domain of your identity provider. This is used to discover the OIDC endpoints automatically via the `.well-known/openid-configuration` endpoint.

Examples:

* Google: `accounts.google.com`
* Microsoft/Azure AD: `login.microsoftonline.com/{tenant-id}/v2.0`
* Auth0: `your-tenant.us.auth0.com`
* Okta: `your-org.okta.com`
* PingOne: `auth.pingone.com/{environment-id}/as`

Use the **Test Connection** button to verify the domain is correct. A successful test displays:

* Issuer URL
* Authorization endpoint
* Token endpoint
* JWKS URI with key count
* Supported signing algorithms

**Client ID**

The OAuth2 client identifier provided by your identity provider when you register your application.

**Client Secret**

The OAuth2 client secret provided by your identity provider. This value is stored securely and masked in the interface.

**Authentication Method**

How credentials are sent to the token endpoint:

* **client\_secret\_post** (default): Client ID and secret sent in the request body
* **client\_secret\_basic**: Client ID and secret sent via HTTP Basic Authentication header
* **none**: No client authentication (for public clients using PKCE only)

Choose the method supported by your identity provider. Most providers support `client_secret_post`.

**Scopes**

The OAuth2 scopes to request during authentication. Enter scopes separated by commas or spaces.

Common scopes:

* `openid` (required for OIDC)
* `profile` - User's name and profile information
* `email` - User's email address
* `groups` - Group memberships (if supported)
* `offline_access` - Request refresh tokens

Default recommendation: `openid profile email`

**Callback URL**

The OAuth2 redirect URI that must be configured in your identity provider. This field is read-only and automatically generated based on your platform URL:

```
https://your-platform.com/oauth/callback
```

Copy this URL and add it to the "Allowed Redirect URIs" (or similar) setting in your identity provider's application configuration.

#### User Provisioning

Controls how users are created and managed when they sign in through this provider.

**Provisioning Mode**

* **Auto Provision** (default): Automatically create new user accounts when users sign in for the first time. User profile information is populated from the identity provider's claims.
* **Match Only**: Only allow sign-in for users that already exist in the platform. New users will be rejected.

**Allowed Domains**

Restrict sign-in to users with email addresses from specific domains. Leave empty to allow all domains.

Examples:

* `company.com` - Only allow users with @company.com emails
* `company.com, subsidiary.com` - Allow multiple domains

This is useful for enterprise deployments where you want to ensure only employees can access the platform.

**Email Trust Mode**

Determines how email verification is handled for users signing in through this provider:

* **Verify with Claim** (default): Trust the email only if the identity provider sends an `email_verified: true` claim. Otherwise, send a verification email.
* **Trust IdP**: Always trust emails from this provider without verification. Use this for enterprise providers where you control the user directory.
* **Always Verify**: Always send a verification email, regardless of the provider's claims. Use this for maximum security.

#### Display Options

Customize how the provider appears on the login page.

**Logo**

Upload a logo image for the provider. This appears on the login button.

* Recommended size: 64x64 pixels (for high-DPI displays)
* Supported formats: PNG, JPG, SVG

**Button Text**

Custom text for the login button. If not specified, defaults to "Login with {Provider Name}".

Examples:

* "Sign in with Google"
* "Company SSO"
* "Employee Login"

#### Attribute Mapping

Map claims from the identity provider's ID token to user profile fields. Most OIDC providers use standard claim names, but some may use custom attributes.

| Platform Field | Default Claim        | Description                  |
| -------------- | -------------------- | ---------------------------- |
| Username       | `preferred_username` | User's unique identifier     |
| Email          | `email`              | User's email address         |
| Name           | `name`               | User's full display name     |
| Picture        | `picture`            | Profile photo URL            |
| Website        | `website`            | User's website               |
| Company        | `organization`       | Company or organization name |
| Location       | `locale`             | User's locale or location    |

Only modify these mappings if your identity provider uses non-standard claim names.

***

## Provider-Specific Setup Guides

### Google

1. Go to the [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select an existing one
3. Navigate to **APIs & Services** → **Credentials**
4. Click **Create Credentials** → **OAuth client ID**
5. Select **Web application**
6. Add your callback URL to **Authorized redirect URIs**
7. Copy the Client ID and Client Secret

**Configuration:**

* Domain: `accounts.google.com`
* Scopes: `openid profile email`

### Microsoft / Azure AD

1. Go to the [Azure Portal](https://portal.azure.com/)
2. Navigate to **Azure Active Directory** → **App registrations**
3. Click **New registration**
4. Enter a name and select the appropriate account types
5. Add your callback URL as a **Redirect URI** (Web platform)
6. After creation, copy the **Application (client) ID**
7. Go to **Certificates & secrets** → **New client secret**
8. Copy the secret value

**Configuration:**

* Domain: `login.microsoftonline.com/{tenant-id}/v2.0`
  * Use `common` for multi-tenant apps
  * Use your specific tenant ID for single-tenant apps
* Scopes: `openid profile email`

### Auth0

1. Go to your [Auth0 Dashboard](https://manage.auth0.com/)
2. Navigate to **Applications** → **Create Application**
3. Select **Regular Web Application**
4. Go to the **Settings** tab
5. Add your callback URL to **Allowed Callback URLs**
6. Copy the Domain, Client ID, and Client Secret

**Configuration:**

* Domain: `your-tenant.us.auth0.com` (or your custom domain)
* Scopes: `openid profile email`

### Okta

1. Go to your [Okta Admin Console](https://your-org-admin.okta.com/)
2. Navigate to **Applications** → **Create App Integration**
3. Select **OIDC - OpenID Connect** and **Web Application**
4. Add your callback URL to **Sign-in redirect URIs**
5. Copy the Client ID and Client Secret

**Configuration:**

* Domain: `your-org.okta.com`
* Scopes: `openid profile email`

### PingOne

1. Go to your [PingOne Admin Console](https://admin.pingone.com/)
2. Navigate to **Connections** → **Applications**
3. Create a new **OIDC Web App**
4. Add your callback URL to the redirect URIs
5. Copy the Client ID and Client Secret
6. Note your Environment ID

**Configuration:**

* Domain: `auth.pingone.com/{environment-id}/as`
* Scopes: `openid profile email`

### Custom OIDC Provider

Any OIDC-compliant identity provider can be configured:

1. Register an application/client in your provider
2. Configure the callback URL as an allowed redirect URI
3. Obtain the Client ID and Client Secret
4. Find the OIDC discovery URL (usually `https://provider/.well-known/openid-configuration`)
5. Extract the domain from the issuer URL

Use the **Test Connection** button to verify your configuration is correct before saving.

***

## Enabling Federated Login

After configuring identity providers, you must enable federated login for users to see the providers on the login page.

1. Navigate to **Settings** → **Accounts**
2. In the **Authentication** section, enable **Federated Login**
3. Select which identity providers should be available
4. Save your settings

The login page will now display the configured providers in the "Or continue with" section.

{% hint style="info" %}
Note: Federated login can also be enabled at the brand level, allowing different brands to have different identity provider configurations.
{% endhint %}

***

## User Experience

### Signing In

When federated login is enabled, users see additional sign-in options on the login page:

1. Click the provider button (e.g., "Sign in with Google")
2. Redirect to the identity provider's login page
3. Authenticate with the provider (may include the provider's own MFA)
4. Redirect back to the platform
5. User is signed in (or account is created if auto-provisioning is enabled)

### Linking Accounts

Existing users can link their account to an identity provider:

1. Sign in with username/password
2. Go to **Account** → **Security**
3. In the **Connected Accounts** section, click to link a provider
4. Authenticate with the provider
5. The account is now linked

### Managing Connected Accounts

Users can view and manage their connected accounts in the Security settings:

* View all linked identity providers
* See the email associated with each provider
* See when each account was linked
* Unlink providers (with restrictions - see below)

#### Unlinking Restrictions

To maintain account access, users cannot unlink an identity provider if:

* It's their only authentication method, AND
* They don't have a password set

Users must either set a password or link another provider before unlinking.

***

## Troubleshooting

### Connection Test Fails

* Verify the domain is correct and accessible
* Check that the OIDC discovery endpoint is publicly accessible
* Ensure there are no firewall rules blocking the connection

### Invalid Client Error

* Verify the Client ID and Client Secret are correct
* Check that the callback URL matches exactly what's configured in the provider
* Ensure the application is not disabled in the provider's console

### User Not Created

* Check the provisioning mode - if set to "Match Only", users must exist beforehand
* Verify allowed domains if domain restrictions are configured
* Check the email trust mode settings

### Claims Not Mapping

* Use the provider's token debugger to inspect the actual claims
* Update attribute mapping to match the provider's claim names
* Ensure requested scopes include the claims you need

### Redirect Loop

* Verify the callback URL is correctly configured in both the platform and provider
* Check for cookie/session issues in the browser
* Ensure HTTPS is properly configured
