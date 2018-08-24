## Eclipse Setup

* download zip from https://www.eclipse.org/downloads/packages/release/photon/r/eclipse-ide-java-developers

* unzip

* start `eclipse.exe`

```
Select a directory as workspace
[x] Use this as the default and do not ask again
Launch

[ ] Always show Welcome at start up
Close Welcome

Window / Preferences
Java / Editor / Save Actions
[x] Perform the selected actions on save
 [x] Format source code
 [x] Organize imports
Apply and Close
```

## Install GWT Plugin
```
Help / Install New Software...
Work with: http://storage.googleapis.com/gwt-eclipse-plugin/v3/release
ENTER

[x] GWT Eclipse Plugin
[x] GWT SDK
Next
Next
(o) I accept the terms of the license agreement
Finish

Security Warning
Install anyway
Do you trust these certificates?
[x] Eclipse.org Foundation
Accept selected

Software Updates
Restart Now
```

## Create GWT Project
```
File / New / Project...
GWT Application / GWT Web Application Project
Next

Project name: whatever
Package: org.whatever
Finish

whatever RIGHT-CLICK / Run As / GWT Development Mode with Jetty

Double-click to open a URL or right-click for more options.
http://127.0.0.1:8888/Whatever.html
```

## First JUnit Test
```
whatever/test RIGHT-CLICK / New / JUnit Test Case
(o) New JUnit 4 test
Package: org.whatever.client
Name: WhateverTest
Finish

JUnit 4 is not on the build path. Do you want to add it?
OK
```
```java
package org.whatever.client;

import com.google.gwt.junit.client.GWTTestCase;

public class WhateverTest extends GWTTestCase {
    public String getModuleName() {
        return "org.whatever.Whatever";
    }

    public void testTheAnswer() {
        assertEquals(42, 6 * 7);
    }
}
```
```
WhateverTest.java RIGHT-CLICK / Run As / GWT JUnit Test
```

## Running multiple tests
```
whatever/test RIGHT-CLICK / Run As / Run Configurations...
```
Due to a bug in the GWT plugin the following error pops up:

> Plug-in `com.gwtplugins.gwt.eclipse.core` was unable to load class `com.google.gwt.eclipse.core.launch.ui.groups.GWTJUnitTabGroup`.

According to https://github.com/gwt-plugins/gwt-eclipse-plugin/issues/168 this bug was fixed in January 2018. Unfortunately, the plugin was released three months earlier and hasn't been updated since :-(

However, the fix https://github.com/ByronLudwig/gwt-eclipse-plugin/commit/ca51039a226fb5fad4db3f0f2e431c6a6eb54c8f is almost trivial:

* exit eclipse

* unzip `eclipse/plugins/com.gwtplugins.gwt.eclipse.core_3.0.0.201710131939.jar` to a temporary folder

* In line 302 of plugin.xml, change `groups` to `tab_groups`

* zip the contents of the temporary folder to a new jar and replace the old one

* run `eclipse.exe -clean` (without the `-clean`, the changes in the `.jar` would go unnoticed!)

> **Side note:** A `.jar` is just a `.zip` with a different name. If your operating system does not handle `.jars` out of the box, simply rename the original `.jar` to `.zip` and the new `.zip` to `.jar`.
