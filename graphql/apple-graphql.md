# Apple GraphQL Schema

## Overview

This conceptual GraphQL schema represents the Apple App Store Connect API and related Apple developer APIs. Apple's public APIs are REST-based (App Store Connect API, Apple Music API, MapKit, Sign in with Apple, CloudKit), but this schema models the same resources and relationships in GraphQL form to enable efficient querying of the Apple developer platform.

Source: https://developer.apple.com/documentation/appstoreconnectapi

## Schema Source

Derived from:
- App Store Connect API OpenAPI specification: https://developer.apple.com/sample-code/app-store-connect/app-store-connect-openapi-specification.zip
- App Store Connect API documentation: https://developer.apple.com/documentation/appstoreconnectapi
- App Store Server API: https://developer.apple.com/documentation/appstoreserverapi
- Apple Music API: https://developer.apple.com/documentation/applemusicapi
- MapKit JS: https://developer.apple.com/documentation/mapkitjs
- Sign in with Apple REST API: https://developer.apple.com/documentation/sign_in_with_apple/sign_in_with_apple_rest_api
- DeviceCheck API: https://developer.apple.com/documentation/devicecheck

## Type Inventory

### App Management
- `App` — Core app record with bundle ID, name, SKU, locale settings, and platform targets
- `AppVersion` — A specific version of an app, including version string, release state, and associated builds
- `AppVersionLocalization` — Locale-specific metadata for an app version (description, keywords, screenshots)
- `AppCategory` — Primary and secondary categories on the App Store
- `AppRelationship` — Relationships between an app and its versions, builds, and beta groups
- `AppAvailability` — Territory and date availability settings for an app
- `AgeRating` — App content rating classification (4+, 9+, 12+, 17+)
- `AppClip` — A lightweight version of an app that launches instantly
- `AppClipDefaultExperience` — The default experience associated with an App Clip invocation

### Pricing
- `AppPrice` — A price tier assignment for an app in a given territory
- `AppPricePoint` — A specific price point within a tier (currency, amount, territory)
- `AppPriceTier` — A named pricing tier (e.g., Tier 1, Tier 2) grouping price points across currencies

### Bundles
- `Bundle` — An app bundle grouping multiple apps sold together
- `BundleId` — A registered bundle identifier (e.g., com.example.app) tied to an Apple Developer team

### Certificates and Profiles
- `Certificate` — A signing certificate (development, distribution, push) issued by Apple
- `Device` — A registered Apple device (iPhone, iPad, Mac) in the developer program
- `DeviceType` — The platform type of a device (IOS, MAC_OS, TVOS, etc.)
- `Profile` — A provisioning profile combining an app ID, certificates, and devices
- `ProvisioningProfile` — Detailed provisioning profile data linking certificates, devices, and entitlements

### Builds and Beta Testing
- `Build` — A compiled binary uploaded to App Store Connect, tied to a version
- `BuildBundle` — A bundle embedded within a build (app extensions, frameworks)
- `BuildBeta` — Beta-specific metadata for a build (expiration, external testing status)
- `BetaGroup` — A group of testers for TestFlight distribution
- `BetaTester` — An individual tester enrolled in TestFlight for one or more apps
- `BetaLicense` — The license agreement presented to beta testers
- `TestFlight` — Aggregate TestFlight configuration and status for an app
- `PreReleaseVersion` — A pre-release version of an app with associated builds
- `InternalTester` — A member of the development team added as an internal tester

### App Review
- `ReviewSubmission` — A submission record for App Store review, including state and version
- `AppReview` — The review record and outcome for a submitted app version
- `AppReviewAttachment` — A file attachment submitted alongside an App Store review
- `AppScreenshot` — A screenshot asset for an app's App Store listing
- `AppPreview` — A video preview asset displayed on an app's App Store page
- `AppTrailer` — A full app preview trailer video for the App Store listing

### In-App Purchases and Subscriptions
- `InAppPurchase` — A consumable, non-consumable, or non-renewing in-app purchase product
- `Subscription` — An auto-renewable subscription product
- `SubscriptionGroup` — A group of related subscription products offered at different durations/prices
- `SubscriptionGroupLocalization` — Locale-specific name and description for a subscription group
- `SubscriptionLocalization` — Locale-specific display name and description for a subscription
- `SubscriptionOffer` — An introductory or promotional offer for a subscription
- `SubscriptionPrice` — The price of a subscription in a given territory and currency

### Analytics and Reports
- `AnalyticsReportRequest` — A request to generate an analytics report for an app
- `AnalyticsReport` — A generated analytics report with download segments
- `FinanceReport` — A financial report covering revenue and proceeds for a reporting period
- `SalesReport` — A sales and trends report for an app or suite of apps

### Users and Roles
- `UserRole` — A role assignment granting a user access to App Store Connect features
- `UserInvitation` — A pending invitation for a new user to join an App Store Connect team
- `User` — A member of an App Store Connect team with one or more roles

### Locales and Territories
- `Territory` — An App Store territory (country/region) where an app is available
- `Locale` — A language/region locale code for localizable content

### Ratings and Reviews
- `Rating` — An individual customer rating (1–5 stars) for an app
- `Review` — A customer review containing written feedback and a star rating
- `RatingSummary` — Aggregate rating statistics (average, count breakdown by star) for an app

### Game Center
- `GameCenter` — Game Center configuration for an app
- `Achievement` — A Game Center achievement with display metadata and point value
- `AchievementLocalization` — Locale-specific name and description for a Game Center achievement
- `Leaderboard` — A Game Center leaderboard definition
- `LeaderboardLocalization` — Locale-specific name and formatting for a leaderboard

### Maps
- `MapKitToken` — A signed JWT token for authenticating MapKit JS and Maps Server API requests
- `GeocodedPlace` — A resolved location result from the Maps Server geocoding endpoint
- `SearchResult` — A place or point-of-interest result from the Maps Server search endpoint
- `EtaResult` — Estimated time of arrival result between origin and destination coordinates

### CloudKit
- `CloudKitRecord` — A record stored in a CloudKit public, private, or shared database
- `CloudKitRecordField` — A field within a CloudKit record (string, integer, asset, location, etc.)
- `CloudKitAsset` — A binary asset (image, file) attached to a CloudKit record

### Sign in with Apple
- `AppleIdentityToken` — The signed JWT identity token returned after Sign in with Apple authentication
- `AppleAuthorizationCode` — The one-time authorization code exchanged for tokens via the REST API
- `AppleRefreshToken` — A long-lived refresh token for Sign in with Apple sessions

### Push Notifications
- `PushNotification` — A notification payload delivered via Apple Push Notification service (APNs)
- `PushNotificationAlert` — The alert component of a push notification (title, body, subtitle)
- `DevicePushToken` — A device-specific APNs push token registered by an app

### Wallet
- `WalletPass` — A pass stored in Apple Wallet (boarding pass, coupon, event ticket, etc.)
- `WalletOrder` — An order tracked in Apple Wallet with status and line items

### DeviceCheck
- `DeviceCheckToken` — A token generated on-device by the DeviceCheck framework
- `DeviceCheckBits` — The two server-controlled bits associated with a device for fraud prevention

## Query Examples

```graphql
# Fetch an app and its latest version localization
query GetApp($appId: ID!, $locale: String!) {
  app(id: $appId) {
    id
    name
    bundleId
    primaryLocale
    latestVersion {
      versionString
      appStoreState
      localizations(locale: $locale) {
        locale
        description
        keywords
        whatsNew
        screenshots {
          imageAsset {
            templateUrl
            width
            height
          }
        }
      }
    }
    categories {
      primary { name }
      secondary { name }
    }
    ageRating
    availability {
      territories { name code }
      availableInNewTerritories
    }
  }
}

# List beta testers in a group
query GetBetaGroup($groupId: ID!) {
  betaGroup(id: $groupId) {
    id
    name
    isInternalGroup
    feedbackEnabled
    testers {
      id
      email
      firstName
      lastName
      betaGroups { name }
    }
    builds {
      version
      processingState
      expirationDate
    }
  }
}

# Get in-app purchases for an app
query GetInAppPurchases($appId: ID!) {
  app(id: $appId) {
    inAppPurchases {
      id
      name
      productId
      inAppPurchaseType
      state
    }
    subscriptionGroups {
      id
      referenceName
      subscriptions {
        id
        name
        productId
        familySharable
        state
        subscriptionPeriod
        prices {
          territory { name }
          startDate
          endDate
        }
      }
    }
  }
}
```

## Authentication

All App Store Connect API queries require a signed JWT (JSON Web Token):

- Algorithm: ES256
- Issuer: Your Issuer ID from App Store Connect
- Key ID: Your private key ID
- Expiration: Maximum 20 minutes
- Audience: `appstoreconnect-v1`

Reference: https://developer.apple.com/documentation/appstoreconnectapi/generating-tokens-for-api-requests
