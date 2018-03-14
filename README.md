Source: [https://christiangomez.me/blog/16/vr-in-the-browser-oh-yes-a-frame/](https://christiangomez.me/blog/16/vr-in-the-browser-oh-yes-a-frame/)

# Introduction
What exactly is WebVR and how does it work? Well, **WebVR** is a web specification that allows you to experience virtual reality from within your browser, pretty cool right? The first adaption of WebVR was introduced in 2014 by a developer at Mozilla. Simply put, WebVR exposes a few APIs that allows developers to interact with VR devices like the HTC Vive, Oculus Rift, and Google Cardboard. The underlying power behind WebVR is **WebGL** which allows us to offset the rendering of graphics to the graphics processor for much better performance. WebVR utilizes WebGL and various other APIs to get VR device information and send frames to them. It also is able to get other information about the device like the controller position and head tracking.

Since there are many different WebVR APIs, A-Frame's goal is to simplify everything by providing pre-built components. At a low level, A-Frame utilizes WebVR APIs and ports them over to a familiar HTML markup structure. This helps developers from having to begin from scratch when starting their WebVR projects and alleviates the need to interact with a bunch of WebVR APIs. Of course, developers still have the option to build from the ground up as the published WebVR draft is [available](https://immersive-web.github.io/webvr/spec/1.1/) which includes all available APIs. A-Frame is definitely the best option for any web developer who wants to get into VR game development.

# Getting Started
Since A-Frame utilizes HTML markup for its structure, we can initialize a scene by simply using the `<a-scene></a-scene>` tag. A **scene** is simply where all your objects are put and rendered on the screen. A-Frame handles all the WebVR boilerplate for us, like camera lighting, rendering loop and other API calls. To initialize an A-Frame project we will just simply include the A-Frame framework into a blank HTML document and include the `a-scene` tag to initialize a basic scene. We will also include a simple box using the `a-box` component that will help us see how items are rendered.

```html
<html>
	<head>
		<title>Hello metaverse!</title>
		<script src="https://aframe.io/releases/0.8.0/aframe.min.js" type="text/javascript"></script>
	</head>
	<body>
		<a-scene>
			<a-box position="0 1 -3" rotation="0 45 0" color="#34495e"></a-box>
		</a-scene>
	</body>
</html>
```
In this scene, we added a very basic box at position `0 1 -3` where 0 is the x axis, 1 is the y axis, and -3 is the z axis. A negative value in the z axis means the object extends in and appears closer where a negative value in the x axis means the object is shifted to the left.  We also rotated the box 45 degress on the y axis and assigned it a color for visibility. Adding components to your scene is very simple and doesn't require much effort at all but what about more complicated objects?
# Adding Objects
Adding simple objects like boxes, and spheres is fun and all, but what about the cool stuff!? The latest standard for efficient transmission of 3d assets and scenes is **glTF**, which aims to provide simplicity of loading 3d models and textures. It's been called the "JPEG of 3D Models" for its portability and versatility. Let's load a basic car glTF model into our A-Frame scene and using the `<a-entity>` object. First, you will need to download the model [here](https://github.com/chr8993/AFrameExample). Then include it in your project directory and add this line within your `<a-scene>` tags.
```html
<a-entity gltf-model="url(car.gltf)"></a-entity>
```
The result should look similar to the image below. By simply adding the `a-entity` tag and specifying the glTF model, the 3d model and textures are loaded without much effort at all. It's pretty awesome how simple the process has become to load 3d models within a web browser.
![AFrame](https://res.cloudinary.com/cinemate/image/upload/v1520804211/frame-_vkdiz5.png)
# Animation
Since the glTF format supports embedded animations we can use an animated glTF model to show how animations work within A-Frame. In order to begin using animations within A-Frame, there is an additional dependency that we will need to include as a script in the document head. We will need to include the **animation-mixer** component from the A-Frame extras library. Lets add the A-Frame extras extension to our document and include an animated cube object in our project. You can download the animated cube model [here](https://github.com/chr8993/AFrameExample) and add it to your project folder. The code should look similar to what is below.
```html
<html>
	<head>
		<title>Hello metaverse!</title>
		<script src="https://aframe.io/releases/0.8.0/aframe.min.js" type="text/javascript"></script>
		<script src="https://cdn.rawgit.com/donmccurdy/aframe-extras/v3.13.1/dist/aframe-extras.min.js"></script>
	</head>
	<body>
		<a-scene>
			<a-entity position="0 0 -5" gltf-model="url(AnimatedCube/AnimatedCube.gltf)" animation-mixer></a-entity>
		</a-scene>
	</body>
</html>
```
The end result should look like the image below. The cube should be spinning immediately after adding the `animation-mixer` attribute to your `a-entity` object. The animated will be looped by default. You can see the live result here [https://chr8993.github.io/AFrameExample](https://chr8993.github.io/AFrameExample/).
![CubeAnimated](https://res.cloudinary.com/cinemate/image/upload/v1520875072/animated_cube_xx52zz.gif)
# Importing Unity Scenes
Importing `obj` files with textures already included allows you to basically create whatever environment you want.  For this example, I've created a Unity script that allows you to export obj files from a Unity scene. Of course, this step is optional since all that will be exported is obj and texture files which can also be found online or created by yourself. But if you have exposure to Unity already which a lot of game developers already do, then this could be very useful for exporting your already built scenes from Unity. The source code and instructions for this script are available on [github](https://github.com/chr8993/AFrameExporter) and I recommend you check it out to see how the internals work. In this example, we will export a simple low poly scene from unity and import it into our A-Frame project.

![AFrame](https://res.cloudinary.com/cinemate/image/upload/w_1000,c_fill/v1520656106/frame-chrome-mac_1_xas7iu.png)
The output of the script will include a folder of `models` and `images` that will contain both our texture files and 3d models. It will also export an `index.html` file that will have our A-Frame markup with all of our components added from the Unity scene. Check out the code below to see how the scene from the image above was structured.
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>A-Frame</title>
    <meta name="description" content="A-Frame">
    <script src="https://aframe.io/releases/0.7.0/aframe.min.js"></script>
  </head>
  <body>
    <a-scene stats="true">
        <a-assets>
          <a-asset-item id="road_CornerLines_obj" src="models/road_CornerLines_mesh1.obj"></a-asset-item>
          <a-asset-item id="road_CornerLines_mtl" src="images/road_CornerLines_mesh1.mtl"></a-asset-item>
          <a-asset-item id="tree_small_mesh_obj" src="models/tree_small_mesh.obj"></a-asset-item>
          <a-asset-item id="tree_small_mesh_mtl" src="images/tree_small_mesh.mtl"></a-asset-item>
          <a-asset-item id="store_small_mesh_obj" src="models/store_small_mesh.obj"></a-asset-item>
          <a-asset-item id="store_small_mesh_mtl" src="images/store_small_mesh.mtl"></a-asset-item>
          <a-asset-item id="hydrant_mesh_obj" src="models/hydrant_mesh.obj"></a-asset-item>
          <a-asset-item id="hydrant_mesh_mtl" src="images/hydrant_mesh.mtl"></a-asset-item>
          <a-asset-item id="lamp_mesh_obj" src="models/lamp_mesh.obj"></a-asset-item>
          <a-asset-item id="lamp_mesh_mtl" src="images/lamp_mesh.mtl"></a-asset-item>
          <a-asset-item id="Prop_Tree_01_obj" src="models/Prop_Tree_01.obj"></a-asset-item>
          <a-asset-item id="Prop_Tree_01_mtl" src="images/Prop_Tree_01.mtl"></a-asset-item>
          <a-asset-item id="bush_large_mesh_obj" src="models/bush_large_mesh.obj"></a-asset-item>
          <a-asset-item id="bush_large_mesh_mtl" src="images/bush_large_mesh.mtl"></a-asset-item>
          <a-asset-item id="car_mesh_obj" src="models/car_mesh.obj"></a-asset-item>
          <a-asset-item id="car_mesh_mtl" src="images/car_mesh.mtl"></a-asset-item>
          <img id="sky_box" src="images/skybox.png" />
        </a-assets>
        <a-entity obj-model="obj: #road_CornerLines_obj;mtl: #road_CornerLines_mtl;" ></a-entity>
        <a-entity obj-model="obj: #tree_small_mesh_obj;mtl: #tree_small_mesh_mtl;" ></a-entity>
        <a-entity obj-model="obj: #store_small_mesh_obj;mtl: #store_small_mesh_mtl;" ></a-entity>
        <a-entity obj-model="obj: #hydrant_mesh_obj;mtl: #hydrant_mesh_mtl;" ></a-entity>
        <a-entity obj-model="obj: #lamp_mesh_obj;mtl: #lamp_mesh_mtl;" ></a-entity>
        <a-entity obj-model="obj: #Prop_Tree_01_obj;mtl: #Prop_Tree_01_mtl;" ></a-entity>
        <a-entity obj-model="obj: #bush_large_mesh_obj;mtl: #bush_large_mesh_mtl;" ></a-entity>
        <a-entity obj-model="obj: #car_mesh_obj;mtl: #car_mesh_mtl;" rotation="0 -90 0" ></a-entity>
        <a-sky color="#3498db"></a-sky>
      </a-scene stats="true">
  </body>
</html>
```
You'll notice that all of our 3d components models and textures have been pre-loaded using the `<a-assets>` tag. This is using the A-Frame asset management system that pre-loads and caches our assets for much better performance. We've also included all of our models using the `<a-entity>` tag that will then reference the appropriate obj model and material pre-defined in the asset management system. You can see the final result here [https://chr8993.github.io/AFrameExample/Final](https://chr8993.github.io/AFrameExample/Final).
