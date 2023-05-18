# MyGeoVerse Content Creation

## Introduction
MyGeoVerse app can visualize 3D contents prepared in **Unity** with **Universal Render Pipeline** (*URP*). Current suggested version is Unity **2021.3.0f1**

> You can use any version of unity which is greater then **2021.3.0f1**

## Asset Creation
### Steps
- [Create new unity project](#create-new-unity-project)
- [Create root content object](#create-root-content-object)
- [Populate your asset](#populate-your-asset)
- [Export unitypackage from Unity](#export-unitypackage-from-unity)
- [Upload to System](#upload-to-system)

### Create new unity project
> You can skip this step if you want to use existing **Unity URP** project
1. Open Unity Hub
2. Click **New Project** on top right of the screen
3. Select correct Unity Version (**2021.3.0f1**)
4. Select **3d Sample Scene URP**
5. Give name to your project and select location
6. Click **Create project** button from bottom right.

![](https://public.3.basecamp.com/p/1xe7HT3CozGKDCCSas3azpzJ/upload/download/create-project.gif)

### Create root content object

Once unity opens created project

1. Create empty scene
2. Right click on the **Hierarchy**
3. Select **Create Empty**
4. Rename as **obj**
5. Reset transform's variables to make is object is positioned correctly
5. Drag created object from **Hierarchy** to **Project** window to create prefab

![](https://public.3.basecamp.com/p/btn7NEC5LiBQ6NrQZKs9bixQ/upload/download/create-root-object.gif)

> Root content object's name must be **obj** otherwise your content won't be recognized as valid!

> There should be only one prefab named as **obj** in your unity package, otherwise system will use first found obj **named** prefab and that my cause problem

### Populate your asset

Now you can add any kind of Unity object to your root obj prefab as child. You can import textures and fbx files to your project and use them to populate your asset. As an example we are going to add some primitive gameobjects to our asset.

1. Locate your prefab within **Project window**
2. Double click on it to open
3. Right click on **Hierarchy** and select **3D Object/Cube**
4. Right click on **Hierarchy** and select **3D Object/Sphere**
5. Adjust **Cube**'s and **Sphere**'s parameters for your liking
6. Hit Ctrl+S on your keyboard to make sure your changes are saved

![](https://public.3.basecamp.com/p/Bf6z5X9qYvVUbKmHHV2XH2ac/upload/download/populate-content.gif)

> Now you are created your content, it is ready to export from unity to XR Masters' system

### Export unitypackage from Unity

1. Locate your prefab within **Project window**
2. Right click on your prefab which you want to export
3. Click **Export Package...**
4. Make sure **Include dependencies** checkbox is selected
5. Click **Export...** button on the bottom right of the popup window
6. Select location within your device to export

![](https://public.3.basecamp.com/p/VH5tqKq1udK1MvQjBE1phR6F/upload/download/export-unitypackage.gif)

> Unity as default will try to include all required asset used within your prefab, but sometimes it can include other asset which aren't required for your content. You can click the deselect checkbox next to those assets to exclude them to optimize your asset's performance

### Upload to System

> You will need to have a MyGeoVerse account for this step. You can create one with [mygeoverse.com](https://mygeoverse.com) or with our mobile app for [Android](https://play.google.com/store/apps/details?id=com.mygeoverse.platform&pli=1) or [iOS](https://apps.apple.com/us/app/mygeoverse/id6443591665)

> Keep in mind unitypackage size should be smaller than 100mb otherwise system won't accept it

1. Open a web browser
2. Go to [mygeoverse.com](https://mygeoverse.com/assets)
3. Click **Create Asset** button on the top right of the page
4. Enter account information to login
5. Give name to your content
6. Give tags to your asset to search it within database easly
7. Click **Select File** button next to **Unity Package** line and select the .unitypackage file you generated in previous step
8. Upload preview image file for asset to locate it more easly
9. Click **Create** button on the bottom of the page to submit your content

![](https://public.3.basecamp.com/p/bjEyCJBdvCFWhPUwkTsXgnDU/upload/download/upload-content.gif)

> Preview image files can be any pixel size but aspect ratio should **1** which means your image file need to have same pixel size for width and height. eg. 512x512
