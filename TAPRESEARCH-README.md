# TapResearch LÖVE SDK for Android

This [repository](https://github.com/TapResearch/love-android/) is forked from https://github.com/love2d/love-android 

This project contains a very basic example written in lua that shows how to use the TapResearch SDK 
in your love2d Android app.

You can also run this example from Android Studio onto an emulator.  

# Example lua game files

There are 3 files in `app/src/embed/assets/`

| File            | Purpose                      |
|-----------------|------------------------------|
| conf.lua        | Sample Configuration File    |
| main.lua        | Sample Game File             |
| tapresearch.lua | Contains the TapResearch API |

 `main.lua` shows how to use the TapResearch API in detail starting with initialization and waiting
for the SDK Ready event.  The API Token and user-identifier will work, but only for demonstration 
 purposes.

# Integration Instructions

To integrate TapResearch into your own Android app, follow these instructions:

## 1. Copy `app/src/embed/assets/tapresearch.lua`

Copy **our** `tapresearch.lua` to **your** src/embed/assets folder. 

## 2. Copy **our** `app/src/main/cpp/love/src/modules/tapresearch` folder 

Copy `app/src/main/cpp/love/src/modules/tapresearch` folder to your `app/src/main/cpp/love/src/modules/` folder.

It will contain a single file: `tapresearch_bindings_android.cpp`

## 3. Manually edit love.cpp

Open **our** `app/src/main/cpp/love/src/modules/love/love.cpp` file.

Search this file for `tapresearch`.  You will find 2 matches:  

```
extern int luaopen_tapresearch_native(lua_State* L);
```

**AND..**

```
static const luaL_Reg modules[] = {
        { "tapresearch_native", luaopen_tapresearch_native },
```        

Add these same blocks to **your own** `app/src/main/cpp/love/src/modules/love/love.cpp`

## 4. Manually edit CMakeLists.txt

Open **our** `app/src/main/cpp/love/CMakeLists.txt` file.

Search for `love_tapresearch_root`.  You will find several matches:

First, you will find the following block:

```
#
# tapresearch
#

add_library(love_tapresearch_root STATIC
src/modules/tapresearch/tapresearch_bindings_android.cpp
)
target_link_libraries(love_tapresearch_root PUBLIC
lovedep::Lua
lovedep::SDL
)
```

**AND..** then you will find another match:

```
set(LIBLOVE_DEPENDENCIES
	love_tapresearch_root
```

Add these blocks to **your own** `app/src/main/cpp/love/CMakeLists.txt` file.

## 5. Copy **our** TapResearchLoveBridge.java

Copy our folder `app/src/main/java/com/tapresearch/love` to your own app src location such that
you will have the same folder structure `app/src/main/java/com/tapresearch/love`.  It will contain
a single file: `TapResearchLoveBridge.java`

## 6. Edit **your** GameActivity.java

In the `onCreate()` method, add the following as the first line:

```
com.tapresearch.love.TapResearchLoveBridge.setActivity(this);
```

It will look like:
```
@Override
protected void onCreate(Bundle savedInstanceState) {
  com.tapresearch.love.TapResearchLoveBridge.setActivity(this);
  ...
  ...
}
```

## 7.  Lastly, edit your `app/build.gradle`

Add TapResearch dependencies to your `dependencies` block:

```
    // required by TapResearch SDK
    implementation 'com.tapresearch:tapsdk:3.7.3--rc1'
    implementation 'org.jetbrains.kotlinx:kotlinx-serialization-json:1.5.0'
    implementation 'androidx.lifecycle:lifecycle-process:2.6.1'
    implementation 'com.google.android.gms:play-services-ads-identifier:18.1.0'
    implementation 'androidx.core:core-ktx:1.10.1'
    implementation 'com.google.android.gms:play-services-appset:16.1.0'
```

It will look something like:

```
dependencies {
    implementation 'androidx.appcompat:appcompat:1.7.0'
    implementation 'com.google.android.material:material:1.12.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.2.0'
    implementation 'androidx.navigation:navigation-fragment:2.8.5'
    implementation 'androidx.navigation:navigation-ui:2.8.5'
    implementation 'androidx.recyclerview:recyclerview:1.3.2'
    implementation 'androidx.swiperefreshlayout:swiperefreshlayout:1.1.0'
    implementation 'com.google.oboe:oboe:1.9.3'

    // required by TapResearch SDK
    implementation 'com.tapresearch:tapsdk:3.7.3--rc1'
    implementation 'org.jetbrains.kotlinx:kotlinx-serialization-json:1.5.0'
    implementation 'androidx.lifecycle:lifecycle-process:2.6.1'
    implementation 'com.google.android.gms:play-services-ads-identifier:18.1.0'
    implementation 'androidx.core:core-ktx:1.10.1'
    implementation 'com.google.android.gms:play-services-appset:16.1.0'

}
```

Note: You should remove any duplicate dependencies and keep the more recent version.

## 8. Add TapResearch functionality to your own app.

By referencing our example `main.lua` file, you can see how to initialize the SDK, wait for SDK 
ready, and show the survey wall.  And much more.