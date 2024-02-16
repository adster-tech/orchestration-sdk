# Adster SDK Implementation Guide for Android

Detailed guide is also available [here](https://docs.adster.tech)

## Introduction
This guide provides a comprehensive step-by-step process for integrating the AdSter SDK into your Android application. Ensure you follow each section carefully for successful implementation.

## Prerequisites
Before beginning, ensure your application's build file adheres to the following requirements

1. minSdkVersion of 24 or higher
2. compileSdkVersion of 33 or higher

## Configuration

### Set up Repository

Open your Android Studio project and navigate to settings.gradle file
Add the following Maven repository configuration inside the dependencyResolutionManagement block


```
repositories {
  ...
  maven {
    url = uri("https://maven.pkg.github.com/adster-tech/orchestration-sdk")
    credentials {
      username = GITHUB_USERNAME
      password = GITHUB_TOKEN
    }
  }
}
```
Replace `GITHUB_USERNAME` and `GITHUB_TOKEN` with your GitHub username and personal access token.


### Add Dependencies

1. Open the **build.gradle (Module: app)** file.
2. Find the dependencies block.
3. Add the SDK dependency.

```gradle
implementation 'com.adster:orchestrationsdk:1.0.2'
```

### Authentication

It's crucial not to hardcode GitHub credentials. Store credentials in `gradle.properties` or use environment variables.

In gradle.properties, add

```properties
 
GITHUB_USERNAME=username
GITHUB_TOKEN=token
```
In settings.gradle, reference these properties:

```gradle
username = findProperty("GITHUB_USERNAME") ?: System.getenv("GITHUB_USERNAME")
password = findProperty("GITHUB_TOKEN") ?: System.getenv("GITHUB_TOKEN")
```
This approach enhances security by keeping credentials out of source control.


## Initialising the SDK

Before loading ads, Make sure to initialize the AdSter Ads SDK by calling `AdSter.INSTANCE.initializeSdk()` which initializes the SDK and calls back a completion listener once initialization is complete. This needs to be done only once, ideally at app launch. Below is an example

### For Java
```java
AdSter.INSTANCE.initializeSdk(getApplicationContext(), adapterStatus -> {
// Initialization completes here
})
```

### For Kotlin 
```kotlin
AdSter.initializeSdk(getApplicationContext(), object : InitializationListener {
  override fun onInitializationComplete(adapterStatus: List<Adapterstatus>) {
 
  }
})
</Adapterstatus>
```

## How to Create an Ad


### Banner Ads

Below are the steps to load and render a banner Ad on your app:

- 1. Create your `AdRequestConfiguration` as per below format

  ```
  val configuration = AdRequestConfiguration(context,"Your_placement_name")
  ```

- 2. Call `loadAd()` method as per below format

For java:

```java
AdSter.INSTANCE.loadAd(configuration, new AdsEventListener() {
  @Override
    public void onBannerAdLoaded(@NonNull MediationBannerAd ad) {
      super.onBannerAdLoaded(ad);
      // Show banner ad here
    }
  @Override
    public void onFailure(@NonNull AdError adError) {
      super.onFailure(adError);
      // Handle failure here
    }
});
```

For kotlin:

```kotlin
AdSter.loadAd(configuration, object : AdsEventListener() {
  override fun onBannerAdLoaded(ad: MediationBannerAd) {
    super.onBannerAdLoaded(ad)
    // Show banner ad here
  }
  override fun onFailure(adError: AdError) {
    // Handle failure here
  }
});
```

- 3. Inside the `onBannerAdLoaded` callback method invoke `getView()` method of `MediationnBannerAd` object to add AdSter banner view to given layout as shown below

```
container.removeAllViews();
container.addView(ad.getView());
```

- 4. Call `MediationBannerAd.destroy()` When activity/fragment is destroyed or detached.


### Interstitial Ads

Below are the steps to load and render a interstitial Ad on your app:

- 1. Create your `AdRequestConfiguration` as per below format

  ```
  val configuration = AdRequestConfiguration(context,"Your_placement_name")
  ```

- 2. Call `loadAd()` method as per below format

For java:

```java
AdSter.INSTANCE.loadAd(configuration, new AdsEventListener() {
   @Override
     public void onInterstitialAdLoaded(@NonNull MediationInterstitialAd ad) {
       super.onInterstitialAdLoaded(ad);
     }
 
   @Override
     public void onFailure(@NonNull AdError adError) {
       // Handle Failure here
     }
});
```

For kotlin:

```kotlin
AdSter.loadAd(configuration, object : AdsEventListener() {
  override fun onInterstitialAdLoaded (ad: MediationInterstitialAd) {
    super. onInterstitialAdLoaded (ad)
    // Show Interstitial ad here
  }
  override fun onFailure(adError: AdError) {
    // Handle failure here
  }
})
```

- 3. Inside the `onInterstitialAdLoaded` callback method invoke `showAd(activity)` method of `MediationInterstitialAd` object to show AdSter interstitial ad above any activity as shown below

For java:

```java
@Override
  public void onInterstitialAdLoaded(@NonNull MediationInterstitialAd ad) {
    super.onInterstitialAdLoaded(ad);
    ad.showAd(activity);
  }
```

For kotlin:

```kotlin
override fun onInterstitialAdLoaded (ad: MediationInterstitialAd) {
  super. onInterstitialAdLoaded (ad)
  ad.showAd(activity);
}
```
Make sure to pass only Activity's context as parameter to `showAd()`

### Rewarded Ads

Below are the steps to load and render a reward Ad on your app:

- 1. Create your `AdRequestConfiguration` as per below format

  ```
  val configuration = AdRequestConfiguration(context,"Your_placement_name")
  ```

- 2. Call `loadAd()` method as per below format

For java:

```java
AdSter.INSTANCE.loadAd(configuration, new AdsEventListener() {
  @Override
    public void onRewardedAdLoaded(@NonNull MediationRewardedAd ad) {
      super.onRewardedAdLoaded(ad);
    }
  @Override
    public void onFailure(@NonNull AdError adError) {
      // Handle Failure here
    }
});
```

For kotlin:

```kotlin
AdSter.loadAd(configuration, object : AdsEventListener() {
  override fun onRewardedAdLoaded (ad: MediationRewardedAd) {
    super. onRewardedAdLoaded (ad)
    // Show Reward ad here
  }
  override fun onFailure(adError: AdError) {
    // Handle failure here
  }
})
```

- 3. Inside the `onRewardedAdLoaded` callback method invoke `showAd(activity)` method of `MediationRewardedAd` object to show AdSter rewarded ad above any activity as shown below

For java:

```java
@Override
  public void onInterstitialAdLoaded(@NonNull MediationRewardedlAd ad) {
    super.onInterstitialAdLoaded(ad);
    ad.showAd(activity);
  }
```

For kotlin:

```kotlin
override fun onRewardedAdLoaded (ad: MediationRewardedAd) {
  super. onRewardedAdLoaded (ad)
  ad.showAd(activity);
}
```
Make sure to pass only Activity's context as parameter to `showAd()`

### Native Ads

Below are the steps to load and render a native Ad on your app:

- 1. Create your `AdRequestConfiguration` as per below format

  ```
  val configuration = AdRequestConfiguration(context,"Your_placement_name")
  ```

- 2. Call `loadAd()` method as per below format

For java:

```java
AdSter.INSTANCE.loadAd(configuration, new AdsEventListener() {
  @Override
    public void onNativeAdLoaded(@NonNull MediationNativeAd ad) {
      super.onNativeAdLoaded(ad);
    }
 
  @Override
    public void onFailure(@NonNull AdError adError) {
      // Handle Failure here
    }
});
```

For kotlin:

```kotlin
AdSter.loadAd(configuration, object : AdsEventListener() {
  override fun onNativeAdLoaded (ad: MediationNativeAd) {
    super. onNativeAdLoaded (ad)
    // Show Reward ad here
  }
  override fun onFailure(adError: AdError) {
    // Handle failure here
  }
})
```

- 3. Inside the `onNativeAdLoaded` callback method use `MediationNativeAd` object to display native ad on your defined layout.

- 4. Define your own native ad layout, below is just an example of a layout:

```xml
   <!--?xml version="1.0" encoding="utf-8"?-->
    <androidx.constraintlayout.widget.constraintlayout xmlns:android="http://schemas.android.com/apk/res/android" xmlns:app="http://schemas.android.com/apk/res-auto" android:layout_width="match_parent" android:layout_height="wrap_content" android:padding="16dp">
 
      <!-- Icon Logo -->
      <imageview android:id="@+id/iconLogoImageView" android:layout_width="54dp" android:layout_height="54dp" android:visibility="gone" app:layout_constraintstart_tostartof="parent" app:layout_constrainttop_totopof="parent" app:layout_constraintbottom_tobottomof="parent">
 
      <!-- choice Logo -->
      <imageview android:id="@+id/choiceImageview" android:layout_width="20dp" android:layout_height="20dp" android:visibility="visible" app:layout_constraintend_toendof="parent" app:layout_constrainttop_totopof="parent">
 
      <framelayout android:id="@+id/mediaView" android:layout_width="54dp" android:layout_height="54dp" android:visibility="gone" app:layout_constraintstart_tostartof="parent" app:layout_constrainttop_totopof="parent" app:layout_constraintbottom_tobottomof="parent">
 
      <androidx.constraintlayout.widget.guideline android:id="@+id/guideline" android:layout_width="wrap_content" android:layout_height="wrap_content" android:orientation="vertical" app:layout_constraintguide_percent="0.2">
 
      <!-- Title -->
      <textview android:id="@+id/titleTextView" android:layout_width="0dp" android:layout_height="wrap_content" android:text="Sample Title" android:textsize="18sp" android:textstyle="bold" app:layout_constraintstart_toendof="@+id/guideline" app:layout_constrainttop_totopof="parent" app:layout_constraintend_toendof="parent">
 
      <!-- Body -->
      <textview android:id="@+id/bodyTextView" android:layout_width="0dp" android:layout_height="wrap_content" android:text="Sample Body Text" android:textsize="14sp" android:layout_marginstart="8dp" app:layout_constraintstart_toendof="@+id/guideline" app:layout_constraintend_toendof="parent" app:layout_constrainttop_tobottomof="@+id/titleTextView">
      <textview android:id="@+id/infoTextView" android:layout_width="wrap_content" android:layout_height="20dp" android:text="Sample advertiser name" android:textsize="15sp" app:layout_constraintstart_toendof="@+id/guideline" app:layout_constraintend_toendof="parent" app:layout_constrainttop_tobottomof="@+id/bodyTextView">
 
      <!-- CTA Button -->
      <button android:id="@+id/ctaButton" android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="Learn More" app:layout_constraintstart_toendof="@+id/guideline" app:layout_constrainttop_tobottomof="@+id/infoTextView" app:layout_constraintbottom_tobottomof="parent" app:layout_constraintend_toendof="parent">
 
     
</button></textview></textview></textview></androidx.constraintlayout.widget.guideline></framelayout></imageview></imageview></androidx.constraintlayout.widget.constraintlayout>
```

- 5. Above sample layout can be used with the `MediationNativeAd` object to render an ad as shown in below example.

For Java:

```java
  private void displayNativeAd(MediationNativeAd ad) {
    // Create AdSter MediationNativeAdView object
    MediationNativeAdView adView = new MediationNativeAdView(this);
 
    // Add this layout as a parent to your native ad layout
    View nativeAdView = LayoutInflater.from(this).inflate(R.layout.ad_native_layout, adView);
 
    // Set native elements
    TextView title = adView.findViewById(R.id.titleTextView);
    TextView body = adView.findViewById(R.id.bodyTextView);
    Button cta = adView.findViewById(R.id.ctaButton);
    ImageView logo = adView.findViewById(R.id.iconLogoImageView);
    ImageView choice = adView.findViewById(R.id.choiceImageview);
    TextView info = adView.findViewById(R.id.infoTextView);
    // MediaView
    FrameLayout mediaView = nativeAdView.findViewById(R.id.mediaView);
 
    // If MediaView is present add AdSter's MediaView as a child to given MediaView
    if (ad.getMediaView() != null) {
        mediaView.addView(ad.getMediaView());
    }
    adView.setBodyView(body);
    adView.setHeadlineView(title);
    adView.setCtaView(cta);
    adView.setLogoView(logo);
    adView.setAdvertiserView(info);
 
    logo.setVisibility(View.VISIBLE);
    // Load logo url using any Image loading library (Glide is just an example here)
    Glide.with(getApplicationContext()).load(ad.getLogo()).into(logo);
 
    // Set native ad elements with data
    title.setText(ad.getHeadLine());
    body.setText(ad.getBody());
    cta.setText(ad.getCallToAction());
    info.setText(ad.getAdvertiser());
 
    Map<string, view=""> map = new HashMap<>();
    map.put("headline", title);
    map.put("body", body);
    map.put("cta", cta);
    map.put("advertiser",info);
    // Send views to AdSter sdk for tracking click/impressionn etc.
    ad.trackViews(adView, null, map);
    // Set MediationNativeAd object
    adView.setNativeAd(ad);
    // Ad native ad view to container
    container.removeAllViews();
    container.addView(adView);
  }
</string,>
```

For Kotlin:

```
  private fun displayNativeAd(ad: MediationNativeAd) {
    // Create AdSter MediationNativeAdView object
    val adView = MediationNativeAdView(this)
    // Add this layout as a parent to your native ad layout
    val nativeAdView: View = LayoutInflater.from(this).inflate(R.layout.ad_native_layout, adView)
    // Set native elements
    val title: TextView = adView.findViewById(R.id.titleTextView)
    val body: TextView = adView.findViewById(R.id.bodyTextView)
    val cta: Button = adView.findViewById(R.id.ctaButton)
    val logo: ImageView = adView.findViewById(R.id.iconLogoImageView)
    val choice: ImageView = adView.findViewById(R.id.choiceImageview)
    val info: TextView = adView.findViewById(R.id.infoTextView)
    // MediaView
    val mediaView: FrameLayout = nativeAdView.findViewById(R.id.mediaView)
 
    // If MediaView is present add AdSter's MediaView as a child to given MediaView
    if (ad.mediaView != null) {
        mediaView.addView(ad.mediaView)
    }
 
    adView.bodyView = body
    adView.headlineView = title
    adView.ctaView = cta
    adView.logoView = logo
    adView.advertiserView = info
    logo.visibility = View.VISIBLE
    // Load logo url using any Image loading library (Glide is just an example here)
    Glide.with(applicationContext).load(ad.logo).into(logo)
 
    // Set native ad elements with data
    title.text = ad.headLine
    body.text = ad.body
    cta.text = ad.callToAction
    info.text = ad.advertiser
 
    val map = HashMap<string, view="">()
    map["headline"] = title
    map["body"] = body
    map["cta"] = cta
    map["advertiser"] = info
 
    // Send views to AdSter sdk for tracking click/impressionn etc.
    ad.trackViews(adView, null, map)
    // Set MediationNativeAd object
    adView.nativeAd = ad
    // Ad native ad view to container
    container.removeAllViews()
    container.addView(adView)
  }
</string,>
```

Make sure to call `trackViews` and `setNativeAd` method before adding `MediationNativeAdView` to the container.


- 6. Call `MediationNativeAd.destroy` when activity or fragment is getting destroyed.


## Event Listeners

To track various lifecycle events of different ad formats, event listeners can be used.

### Banner and Native Ad

For Java:

```java
AdSter.INSTANCE.setAdListener(new AdEventsCallback() {
  @Override
    public void onAdImpression() {
      // Called when Ad is getting impression  
    }
 
  @Override
    public void onAdClicked() {
      // Called when Ad is clicked
    }
});
```

For Kotlin:

```kotlin
AdSter.setAdListener(object : AdEventsCallback() {
  override fun onAdImpression() {
    // Called when Ad is getting impression  
  }
  override fun onAdClicked() {
    // Called when Ad is clicked
  }
})
```

### Interstitial Ad

For Java:

```java
AdSter.INSTANCE.setInterstitialAdListener(new InterstitialAdEventsCallback() {
  @Override
    public void onAdClicked() {
      // Called when Ad is clicked
    }
 
  @Override
    public void onAdImpression() {
      // Called when Ad is getting impression  
    }
 
  @Override
    public void onAdOpened() {
      // Called when Ad is opened
    }
 
  @Override
    public void onAdClosed() {
      // Called when Ad is closed
    }
});
```

For Kotlin:

```kotlin
AdSter.setInterstitialAdListener(object : InterstitialAdEventsCallback() {
  override fun onAdClicked() {
    // Called when Ad is clicked
  }
  override fun onAdImpression() {
    // Called when Ad is getting impression  
  }
  override fun onAdOpened() {
    // Called when Ad is opened
  }
  override fun onAdClosed() {
    // Called when Ad is closed
  }
});
```

### Rewarded Ad

For Java:

```java
AdSter.INSTANCE.setRewardedAdListener(new RewardedAdEventsCallback() {
  @Override
    public void onUserEarnedReward(@NonNull Reward reward) {
      // Called when user is eligible for a reward
    }
 
  @Override
    public void onAdClicked() {
      // Called when ad is clicked
    }
 
  @Override
    public void onAdImpression() {
      // Called when ad is getting impression
    }
 
  @Override
    public void onVideoComplete() {
      // Called when rewarded video completed
    }
 
  @Override
    public void onVideoStart() {
      // Called when rewarded video started.
    }
});
```

For Kotlin:

```kotlin
AdSter.setRewardedAdListener(object : RewardedAdEventsCallback() {
  override fun onUserEarnedReward(@NonNull reward: Reward) {
    // Called when user is eligible for a reward
  }
  override fun onAdClicked() {
    // Called when ad is clicked
  }
  override fun onAdImpression() {
    // Called when ad is getting impression
  }
  override fun onVideoComplete() {
    // Called when rewarded video completed
  }
  override fun onVideoStart() {
    // Called when rewarded video started.
  }
});
```


## App open (Beta)

Adster sdk also gives the option to load and show AppOpen ad format which is the proprietary Ad format of google ad manager and AdMob. Implementation is similar to given in this document https://developers.google.com/admob/android/app-open. Instead of creating a new `AppOpenAdManager` mentioned in the document use method below provided by AdSter sdk and follow the implementation on Activity and Application class just like the document.
AdSter sdk provides methods to show and load app open ads.

### Load App Open Ad

Pass activity's context and your placement name as arguments to `loadAppOpenAd` method

For Java:

```java
AdSter.INSTANCE.loadAppOpenAd(activity, new AdRequestConfiguration(activity, "Your_placement"), new AppOpenAdEventsListener() {
  @Override
    public void onAdLoaded() {
      super.onAdLoaded();
      // Here ad can be shown using a call to showAdIfAvailable method
    }
 
  @Override
    public void onAdLoadFailure(@NonNull AdError adError) {
      super.onAdLoadFailure(adError);
    }
});
```

For Kotlin:

```kotlin
AdSter.loadAppOpenAd(
  activity,
  AdRequestConfiguration(activity, "Your_placement"),
  object : AppOpenAdEventsListener() {
    override fun onAdLoaded() {
        super.onAdLoaded()
        // Here ad can be shown using a call to showAdIfAvailable method
    }
 
    override fun onAdLoadFailure(adError: AdError) {
        super.onAdLoadFailure(adError)
    }
})
```

### Show AppOpen Ad

Pass activity's context and your placement name as arguments to `loadAppOpenAd` method

For Java:

```java
AdSter.INSTANCE.showAdIfAvailable(activity, new AdRequestConfiguration(activity, "your_placement_id"), new AppOpenAdEventsListener() {
  @Override
    public void onShowAdComplete() {
      super.onShowAdComplete();
    }
 
  @Override
    public void onAdLoadFailure(@NonNull AdError adError) {
      super.onAdLoadFailure(adError);
    }
});
```

For Kotlin:

```kotlin
AdSter.showAdIfAvailable(
  activity,
  AdRequestConfiguration(activity, "your_placement_id"),
  object : AppOpenAdEventsListener() {
    override fun onShowAdComplete() {
      super.onShowAdComplete()
    }
    fun onAdLoadFailure(adError: AdError?) {
      super.onAdLoadFailure(adError!!)
    }
});
```

This method will try to show an app open ad if already available. If not it calls loadAd internally to load an app open ad and wherever a new app open event occurs this method immediately shows the ad. 
