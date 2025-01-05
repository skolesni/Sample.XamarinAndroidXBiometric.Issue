# Sample .NET MAUI 8.0 Android Project Demonstrating Xamarin.AndroidX.Biometric Linking Conflict

## Overview

This repository provides a sample *.NET MAUI 8.0* Android project that exhibits a Java linking conflict. The conflict arises upon adding the following NuGet package:

```xml
<PackageReference Include="Xamarin.AndroidX.Biometric" Version="1.1.0.25" />
```

It is presumably supported in *.NET MAUI 8.0* given that Microsoft has updated the `Xamarin.AndroidX.*` dependencies for .NET 8.0. Community implementations of MAUI biometrics, such as [Maui.Biometric](<https://github.com/oscoreio/Maui.Biometric>) and [Plugin.Maui.Biometric](https://github.com/FreakyAli/Plugin.Maui.Biometric), further indicate that `Xamarin.AndroidX.Biometric` usage is intended to be compatible with *.NET MAUI 8.0*.

## Sample Project Setup

- Created with Visual Studio for Mac `17.6.4 (build 413)` (which is now retired).
- The sample application builds and runs correctly without the `Xamarin.AndroidX.Biometric` package.
- The final commit introduces `Xamarin.AndroidX.Biometric` into the project, triggering the Java linking error described below.

## Community workarounds

Several community solutions and workarounds suggest including additional references:

```xml
<PackageReference Include="Xamarin.AndroidX.Collection" Version="1.4.5.1" />
<PackageReference Include="Xamarin.AndroidX.Collection.Ktx" Version="1.4.5.1" />
<PackageReference Include="Xamarin.AndroidX.Biometric" Version="1.1.0.26" />
```

For instance, see:

- [Maui.Biometric/Maui.Biometric.csproj](https://github.com/oscoreio/Maui.Biometric/blob/main/src/libs/Maui.Biometric/Maui.Biometric.csproj)
- [Plugin.Maui.Biometric/Plugin.Maui.Biometric.csproj](https://github.com/FreakyAli/Plugin.Maui.Biometric/blob/master/Plugin.Maui.Biometric/Plugin.Maui.Biometric/Plugin.Maui.Biometric.csproj)
- [Adding `Xamarin.AndroidX.Biometric` in .NET MAUI Library throws an exception](https://stackoverflow.com/questions/77929158/adding-xamarin-androidx-biometric-in-net-maui-library-throws-an-exception)

Despite adding these references, the solution remains unresolved in this example.

## Environment

- **Operating System**: Apple Macbook Pro M1 running macOS Sequoia 15.2, also tested on Azure Pipelines hosted macOS agents
- **.NET SDK Version**: 8.0.403 (see full .NET --info in the original file)
- **Android Development Environment**:
  - Java SDK 17.0.12
  - `platforms/android-34, build-tools/34.0.0`, and other required components installed

See the full environment detail and status here: [Development Environment](./docs/DevelopmentEnvironment.md)

## Adding Xamarin.AndroidX.Biometric Dependency

Including the package results in the following additional references being pulled in:

- Xamarin.AndroidX.Activity
- Xamarin.AndroidX.Annotation
- Xamarin.AndroidX.Annotation.Experimental
- Xamarin.AndroidX.Annotation.Jvm
- Xamarin.AndroidX.AppCompat
- Xamarin.AndroidX.AppCompat.AppCompatResources
- Xamarin.AndroidX.Arch.Core.Common
- Xamarin.AndroidX.Arch.Core.Runtime
- Xamarin.AndroidX.Biometric
- Xamarin.AndroidX.Collection
- Xamarin.AndroidX.Collection.Jvm
- Xamarin.AndroidX.Concurrent.Futures
- Xamarin.AndroidX.Core
- Xamarin.AndroidX.Core.Core.Ktx
- Xamarin.AndroidX.CursorAdapter
- Xamarin.AndroidX.CustomView
- Xamarin.AndroidX.DrawerLayout
- Xamarin.AndroidX.Emoji2
- Xamarin.AndroidX.Emoji2.ViewsHelper
- Xamarin.AndroidX.Fragment
- Xamarin.AndroidX.Interpolator
- Xamarin.AndroidX.LifecycleCommon
- Xamarin.AndroidX.Lifecycle.Common.Jvm
- Xamarin.AndroidX.Lifecycle.LiveData.Core
- Xamarin.AndroidX.Lifecycle.Process
- Xamarin.AndroidX.Lifecycle.Runtime
- Xamarin.AndroidX.Lifecycle.Runtime.Android
- Xamarin.AndroidX.Lifecycle.ViewModel
- Xamarin.AndroidX.Lifecycle.ViewModel.Android
- Xamarin.AndroidX.Lifecycle.ViewModelSavedState
- Xamarin.AndroidX.Loader
- Xamarin.AndroidX.ProfileInstaller.ProfileInstaller
- Xamarin.AndroidX.ResourceInspection.Annotation
- Xamarin.AndroidX.SavcedState
- Xamarin.AndroidX.Startup.StartupRuntime
- Xamarin.AndroidX.Tracing.Tracing
- Xamarin.AndroidX.VectorDrawable
- Xamarin.AndroidX.VectorDrawable.Animated
- Xamarin.AndroidX.VisionParcelable
- Xamarin.AndroidX.ViewPager
- Xamarin.Google.Guava.ListenableFuture
- Xamarin.Jetbrains.Annotations
- Xamarin.Kotlin.StdLib
- Xamarin.KotlinX.AtomicFU
- Xamarin.KotlinX.AtomicFU.Jvm
- Xamarin.KotlinX.Coroutines.Android
- Xamarin.KotlinX.Coroutines.Core
- Xamarin.KotlinX.Coroutines.Core.Jvm

The console output shows no direct compatibility issues: [Package Console Output](./docs/PackageConsoleOutput.md)

## Build Failure

When attempting to build, the process fails with a repeated “type is defined multiple times” error. The relevant excerpt:

```text
Type androidx.collection.ArrayMapKt is defined multiple times:
.../xamarin.androidx.collection.jvm/...ArrayMapKt.class,
.../xamarin.androidx.collection.ktx/...ArrayMapKt.class
```

This triggers the compilation to abort:

```text
Error JAVA0000: Error in .../androidx.collection.collection-jvm.jar:androidx/collection/ArrayMapKt.class:
Type androidx.collection.ArrayMapKt is defined multiple times
...
Compilation failed
...
```

The error detail:

```text
/Users/stanislavkolesnik/Development/Fundsmith/Sample.XamarinAndroidXBiometric.Issue/Sample.XamarinAndroidXBiometric.Issue: Error JAVA0000: Error in /Users/stanislavkolesnik/.nuget/packages/xamarin.androidx.collection.jvm/1.4.5.1/buildTransitive/net8.0-android34.0/../../jar/androidx.collection.collection-jvm.jar:androidx/collection/ArrayMapKt.class:
Type androidx.collection.ArrayMapKt is defined multiple times: /Users/stanislavkolesnik/.nuget/packages/xamarin.androidx.collection.jvm/1.4.5.1/buildTransitive/net8.0-android34.0/../../jar/androidx.collection.collection-jvm.jar:androidx/collection/ArrayMapKt.class, /Users/stanislavkolesnik/.nuget/packages/xamarin.androidx.collection.ktx/1.2.0.9/buildTransitive/net6.0-android31.0/../../jar/androidx.collection.collection-ktx.jar:androidx/collection/ArrayMapKt.class
Compilation failed
java.lang.RuntimeException: com.android.tools.r8.CompilationFailedException: Compilation failed to complete, origin: /Users/stanislavkolesnik/.nuget/packages/xamarin.androidx.collection.jvm/1.4.5.1/buildTransitive/net8.0-android34.0/../../jar/androidx.collection.collection-jvm.jar
androidx/collection/ArrayMapKt.class
 at com.android.tools.r8.utils.S0.a(R8_8.2.33_429c93fd24a535127db6f4e2628eb18f2f978e02f99f55740728d6b22bef16dd:135)
 at com.android.tools.r8.D8.main(R8_8.2.33_429c93fd24a535127db6f4e2628eb18f2f978e02f99f55740728d6b22bef16dd:5)
Caused by: com.android.tools.r8.CompilationFailedException: Compilation failed to complete, origin: /Users/stanislavkolesnik/.nuget/packages/xamarin.androidx.collection.jvm/1.4.5.1/buildTransitive/net8.0-android34.0/../../jar/androidx.collection.collection-jvm.jar:androidx/collection/ArrayMapKt.class
 at Version.fakeStackEntry(Version_8.2.33.java:0)
 at com.android.tools.r8.T.a(R8_8.2.33_429c93fd24a535127db6f4e2628eb18f2f978e02f99f55740728d6b22bef16dd:5)
 at com.android.tools.r8.utils.S0.a(R8_8.2.33_429c93fd24a535127db6f4e2628eb18f2f978e02f99f55740728d6b22bef16dd:82)
 at com.android.tools.r8.utils.S0.a(R8_8.2.33_429c93fd24a535127db6f4e2628eb18f2f978e02f99f55740728d6b22bef16dd:32)
 at com.android.tools.r8.utils.S0.a(R8_8.2.33_429c93fd24a535127db6f4e2628eb18f2f978e02f99f55740728d6b22bef16dd:31)
 at com.android.tools.r8.utils.S0.b(R8_8.2.33_429c93fd24a535127db6f4e2628eb18f2f978e02f99f55740728d6b22bef16dd:2)
 at com.android.tools.r8.D8.a(R8_8.2.33_429c93fd24a535127db6f4e2628eb18f2f978e02f99f55740728d6b22bef16dd:42)
 at com.android.tools.r8.D8.b(R8_8.2.33_429c93fd24a535127db6f4e2628eb18f2f978e02f99f55740728d6b22bef16dd:13)
 at com.android.tools.r8.D8.a(R8_8.2.33_429c93fd24a535127db6f4e2628eb18f2f978e02f99f55740728d6b22bef16dd:40)
 at com.android.tools.r8.utils.S0.a(R8_8.2.33_429c93fd24a535127db6f4e2628eb18f2f978e02f99f55740728d6b22bef16dd:122)
 ... 1 more
Caused by: com.android.tools.r8.utils.b: Type androidx.collection.ArrayMapKt is defined multiple times: /Users/stanislavkolesnik/.nuget/packages/xamarin.androidx.collection.jvm/1.4.5.1/buildTransitive/net8.0-android34.0/../../jar/androidx.collection.collection-jvm.jar:androidx/collection/ArrayMapKt.class, /Users/stanislavkolesnik/.nuget/packages/xamarin.androidx.collection.ktx/1.2.0.9/buildTransitive/net6.0-android31.0/../../jar/androidx.collection.collection-ktx.jar:androidx/collection/ArrayMapKt.class
 at com.android.tools.r8.utils.Q2.a(R8_8.2.33_429c93fd24a535127db6f4e2628eb18f2f978e02f99f55740728d6b22bef16dd:21)
 at com.android.tools.r8.utils.D2.a(R8_8.2.33_429c93fd24a535127db6f4e2628eb18f2f978e02f99f55740728d6b22bef16dd:54)
 at com.android.tools.r8.utils.D2.a(R8_8.2.33_429c93fd24a535127db6f4e2628eb18f2f978e02f99f55740728d6b22bef16dd:10)
 at java.base/java.util.concurrent.ConcurrentHashMap.merge(ConcurrentHashMap.java:2048)
 at com.android.tools.r8.utils.D2.a(R8_8.2.33_429c93fd24a535127db6f4e2628eb18f2f978e02f99f55740728d6b22bef16dd:6)
 at com.android.tools.r8.graph.m4$a.d(R8_8.2.33_429c93fd24a535127db6f4e2628eb18f2f978e02f99f55740728d6b22bef16dd:6)
 at com.android.tools.r8.dex.c.a(R8_8.2.33_429c93fd24a535127db6f4e2628eb18f2f978e02f99f55740728d6b22bef16dd:61)
 at com.android.tools.r8.dex.c.a(R8_8.2.33_429c93fd24a535127db6f4e2628eb18f2f978e02f99f55740728d6b22bef16dd:12)
 at com.android.tools.r8.dex.c.a(R8_8.2.33_429c93fd24a535127db6f4e2628eb18f2f978e02f99f55740728d6b22bef16dd:9)
 at com.android.tools.r8.D8.a(R8_8.2.33_429c93fd24a535127db6f4e2628eb18f2f978e02f99f55740728d6b22bef16dd:45)
 at com.android.tools.r8.D8.d(R8_8.2.33_429c93fd24a535127db6f4e2628eb18f2f978e02f99f55740728d6b22bef16dd:17)
 at com.android.tools.r8.D8.c(R8_8.2.33_429c93fd24a535127db6f4e2628eb18f2f978e02f99f55740728d6b22bef16dd:69)
 at com.android.tools.r8.utils.S0.a(R8_8.2.33_429c93fd24a535127db6f4e2628eb18f2f978e02f99f55740728d6b22bef16dd:28)
 ... 6 more
 (JAVA0000) (Sample.XamarinAndroidXBiometric.Issue) java
```

Further details are available in [Build Output](./docs/BuildOutput.md)
